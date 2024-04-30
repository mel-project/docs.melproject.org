# Implement

In this page, we go over the details of actually implementing a Gibbername library in Rust.

:man\_technologist: [**Follow along on GitHub!**](https://github.com/mel-project/gibbername)&#x20;

{% hint style="info" %}
In the future, cross-language `melprot` bindings will make it possible to implement protocol libraries in other languages, including in-browser JavaScript!

For now, though, Rust is the only supported language.
{% endhint %}

## Project setup

The first step of implementing Gibbername is to create a new Rust library with the `melprot` dependency:

```shell-session
cargo new --lib gibbername
cd gibbername
```

We'll also be adding some dependencies. These will show up in the `Cargo.toml`:

```shell-session
cargo add futures-util anyhow gibbercode hex melprot melstructs stdcode tmelcrypt
```

## Looking up names

The easiest part of Gibbername is looking up the names. This consists of three parts:

* **Decoding the Gibbername** into a _blockchain location_ identifying the start of the Catena chain. This means a block height and a transaction hash.
* **Obtaining and validating the start transaction** by obtaining a snapshot at the given block height, retrieving the start transaction, and making sure that its `data` field says `"gibbername-v1"`.
* **Traversing the Catena chain**, following all the custom-token coins, traverse the Catena chain to the most recent element. We'll then have our binding!

### The Gibbername encoding

How can we squeeze a blockchain location — which identifies a transaction and its location — into a short "gibberish string" like `xoxqax-lobteh`? After all, unique transaction hashes are very long and unwieldy.

Instead, we encode a unique blockchain location as two numbers: the _block height_ and the _transaction position_. This position is the 0-indexed position of the transaction within all the transactions in that block sorted by hash.

This lets us represent any transaction in the blockchain uniquely with two smallish numbers. For instance, the transaction with the "smallest" hash in block `100000` would be represented as `100000,0`.

We then need to represent this pair of numbers as a friendly Gibbername. Fortunately, we can use `gibbercode`, a crate that encodes a pair of numbers into a gibberish string using the consonants for the first number and the vowels for the second.

{% code overflow="wrap" lineNumbers="true" %}
```rust
use melstructs::{Address, BlockHeight, CoinData, CoinValue, Denom, Transaction, TxHash};

/// Decodes a gibbername into a blockchain location.
fn decode_gibbername(gname: &str) -> anyhow::Result<(BlockHeight, u32)> {
    let (height, index) = gibbercode::decode(gname);
    Ok((BlockHeight(height as u64), index as u32))
}

/// Encodes the given height and index into a gibbername.
fn encode_gibbername(height: BlockHeight, index: u32) -> anyhow::Result<String> {
    Ok(gibbercode::encode(
        u128::try_from(height.0)?,
        u128::try_from(index)?,
    ))
}
```
{% endcode %}

### Validating the start transaction

Once we have the blockchain location, we need to retrieve the start transaction. This can be done using `melprot`'s `Snapshot::get_transaction_by_posn()` function.

The start transaction should have a `data` field that says `"gibbername-v1"`, as well as one, and just one, output with denomination `Denom::NewCustom`, and that output must have value `1`. This is the way we ensure that a given Gibbername is actually valid.

{% code overflow="wrap" lineNumbers="true" %}
```rust
/// Gets and validates the starting transaction of the gibbername chain.
/// Validation involves checking the transaction for the following properties:
/// 1. The `data` field says "gibbername-v1"
/// 2. The transaction has a single output with the [themelio_structs::Denom::NewCoin] denomination
///    with a value of 1
async fn get_and_validate_start_tx(
    client: &melprot::Client,
    gibbername: &str,
) -> anyhow::Result<(BlockHeight, TxHash)> {
    let (height, index) = decode_gibbername(gibbername).expect("failed to decode {gibbername}");
    let snapshot = client.snapshot(height).await?;
    let txhash = snapshot.get_transaction_by_posn(index as usize).await?;

    // validate the transaction now
    if let Some(txhash) = txhash {
        let tx = snapshot
            .get_transaction(txhash)
            .await?
            .expect("expected transaction to exist, because txhash exists");

        // check the data
        if &tx.data[..] != b"gibbername-v1" {
            anyhow::bail!("invalid data in the start transaction: {:?}", tx.data);
        }

        let new_outputs = tx
            .outputs
            .iter()
            .filter(|output| output.denom == Denom::NewCustom)
            .collect::<Vec<&CoinData>>();
        if new_outputs.len() == 1 && new_outputs[0].value == CoinValue(1) {
            Ok((height, tx.hash_nosigs()))
        } else {
            anyhow::bail!("invalid start transaction outputs");
        }
    } else {
        anyhow::bail!("could not find starting transaction for the given gibbername: {gibbername}");
    }
}
```
{% endcode %}

### Traversing the Catena chain

Finally, we can traverse the Catena chain to get the coin containing the final binding:

{% code overflow="wrap" lineNumbers="true" %}
```rust
use anyhow::Context;
use futures_util::StreamExt;

async fn traverse_catena_chain(
    client: &melprot::Client,
    start_height: BlockHeight,
    start_txhash: TxHash,
) -> anyhow::Result<CoinData> {

    // First, we get a collection of transactions from our starting height and txhash.
    // We also include a closure that tells us to look for the transaction output that follow our Gibbername rules (a Denom that's either Custom(<start_txhash>) or NewCustom)
    let traversal = client
        .traverse_fwd(start_height, start_txhash, move |tx: &Transaction| {
            tx.outputs.iter().position(|coin_data| {
                (tx.hash_nosigs() == start_txhash && coin_data.denom == Denom::NewCustom)
                    || coin_data.denom == Denom::Custom(start_txhash)
            })
        })
        .expect("failed to traverse forward")
        .collect::<Vec<Transaction>>()
        .await;

    // If the traversal is empty, it means either:
    // 1. The current height and txhash represent the end of the traversal
    // 2. We couldn't find anything for the given height and txhash
    if traversal.is_empty() {
        let snap = client.snapshot(start_height).await?;
        let tx = snap
            .get_transaction(start_txhash)
            .await?
            .context("No transaction with given hash")?;
        let coin = tx
            .outputs
            .iter()
            .find(|coin| coin.denom == Denom::NewCustom);

        match coin {
            Some(coin_data) => return Ok(coin_data.clone()),
            None => anyhow::bail!("No valid gibbercoins found"),
        }
    }

    // Return the last coin in the traversal if it exists
    let last_tx = traversal.last().expect("the traversal is empty");
    if let Some(last_tx_coin) = last_tx
        .outputs
        .iter()
        .find(|coin_data| coin_data.denom == Denom::Custom(start_txhash))
    {
        Ok(last_tx_coin.clone())
    } else {
        anyhow::bail!("the name was permanently deleted");
    }
}

```
{% endcode %}

We can now easily build the gibbername lookup function!

{% code overflow="wrap" lineNumbers="true" %}
```rust
/// Returns the data bound to the given gibbername if there is any.
pub async fn lookup(client: &melprot::Client, gibbername: &str) -> anyhow::Result<String> {
    let (start_height, start_txhash) = get_and_validate_start_tx(client, gibbername).await?;
    let last_coin = traverse_catena_chain(client, start_height, start_txhash).await?;
    let binding = String::from_utf8_lossy(&last_coin.additional_data);

    Ok(binding.into_owned())
}
```
{% endcode %}

## Registering names

Registering names is a little different: we need to _send_ a transaction into the blockchain rather than just reading existing data.

One possible way is to craft a transaction inside our library and send it by directly calling an RPC method on a full node (through something like `melprot::Client::raw_rpc()`). But this is hard, because we must somehow get hold of $MEL to pay transaction fees (possibly by asking the user to send money to some address?). Furthermore, even once we have $MEL, managing the money and the private keys securing it difficult, security-critical task.

Instead, we **ask the user's wallet to send a transaction for us**, and we simply wait until the user finishes doing so. In summary, here are the steps to register a new gibbername:

### Prompt and wait for the transaction

We can now write a function to send the transaction and wait for it to commit in the blockchain.

{% code overflow="wrap" lineNumbers="true" %}
```rust
pub async fn register(
    client: &melprot::Client,
    address: Address,
    initial_binding: &str,
    wallet_name: &str,
) -> anyhow::Result<String> {
    let height = client.latest_snapshot().await?.current_header().height;
    let cmd = register_name_cmd(wallet_name, address, initial_binding)?;
    println!("Send this command with your wallet: {}", cmd);

    // scan through all transactions involving this address, starting at the block height right before we asked the user to send the transacton
    let mut stream = client.stream_transactions_from(height, address).boxed();
    while let Some((transaction, height)) = stream.next().await {
        if &transaction.data[..] == b"gibbername-v1" {
            let txhash = transaction.hash_nosigs();
            let (posn, _) = client
                .snapshot(height)
                .await?
                .current_block()
                .await?
                .abbreviate()
                .txhashes
                .iter()
                .enumerate()
                .find(|(_, hash)| **hash == txhash)
                .expect("No transaction with matching hash in this block.");

            let gibbername = encode_gibbername(height, posn as u32)?;
            return Ok(gibbername);
        }
    }
    unreachable!()
}
```
{% endcode %}

```rust
// A small helper function to create the wallet command for registering a name.
fn register_name_cmd(
    wallet_path: &str,
    address: Address,
    initial_binding: &str,
) -> anyhow::Result<String> {
    let cmd = format!(
        "melwallet-cli --wallet-path {} send --to {},{},{},\"{}\" --hex-data {}",
        wallet_path,
        address,
        0.000001,
        "\"(NEWCUSTOM)\"",
        hex::encode(initial_binding),
        hex::encode("gibbername-v1")
    );

    Ok(cmd)
}
```

When this function is called, the user will be prompted to manually send a transaction with our wallet CLI: `melwallet-cli`. We will continuously stream incoming transactions until we find the one we sent. We are now able to register a name with an arbitrary binding! :rocket:

## Transferring names

Transferring names is left as an exercise to the reader.

{% hint style="info" %}
**Hint**: you'll need to construct a wallet to extend the Catena chain and prompt the user, just like with registration. If you're truly stuck, there's always [our GitHub example](https://github.com/mel-project/gibbername) code :smile:
{% endhint %}

<!-- {% hint style="info" %}
[**Wallet URIs**](https://forum.melproject.org/t/some-thoughts-on-wallet-apis-and-payment-protocols/55#option-3-specify-a-wallet-protocol-around-a-custom-url-scheme-5) are still under construction, but they will replace the current UX of asking the user to type a `melwallet-cli`  command.&#x20;

Here's a quick preview of what they will look like:

* Construct a _wallet URI_ using the standard `melwallet:` URI scheme, describing the transaction we need the user to send
* Prompt the user to "open" this URI with their wallet
  * Right now, we will prompt the user to do so manually.
  * Eventually, a graphical "Gibbername registrar app" should integrate with OS-specific URI-opening functionality.
* Wait for the transaction to commit and derive a gibbername from the location at which the transaction was committed
{% endhint %} -->
