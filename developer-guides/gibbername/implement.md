# Implement

In this page, we go over the details of actually implementing a Gibbername library in Rust.

{% hint style="info" %}
In the future, cross-language `melprot` bindings will make it possible to implement protocol libraries in other languages, including in-browser JavaScript!

For now, though, Rust is the only supported language.
{% endhint %}

## Project setup

The first step of implementing Gibbername is to create a new Rust library with the `melprot` dependency:

```shell-session
$ cargo new --lib gibbername
     Created library `gibbername` package
$ cd gibbername
$ cargo add melprot
    Updating crates.io index
    Adding melprot v0.1.0 to dependencies.
```

We'll also be using the future-combinators from the `futures-util` library:

```shell-session
$ cargo add futures-util
    Updating crates.io index
      Adding futures-util v0.3.26 to dependencies.
```

## Looking up names

The easiest part of Gibbername is looking up the names. This consists of three parts:

- **Decoding the Gibbername** into a _blockchain location_ identifying the start of the Catena chain. This means a block height and a transaction hash.
- **Obtaining and validating the start transaction** by obtaining a snapshot at the given block height, retrieving the start transaction, and making sure that its `data` field says `"gibbername-v1"`.
- **Traversing the Catena chain**, following all the custom-token coins, traverse the Catena chain to the most recent element. We'll then have our binding!

### The Gibbername encoding

How can we squeeze a blockchain location --- which identifies a transaction and its location --- into a short "gibberish string" like `xoxqax-lobteh`? After all, unique transaction hashes are very long and unwieldy.

Instead, we encode a unique blockchain location as two numbers: the _block height_ and the _transaction position_. This position is the 0-indexed position of the transaction within all the transactions in that block sorted by hash.

This lets us represent any transaction in the blockchain uniquely with two smallish numbers. For instance, the transaction with the "smallest" hash in block `100000` would be represented as `100000,0`.

We then need to represent this pair of numbers as a friendly gibbername. Fortunately, we can use `gibbercode`, a crate that encodes a pair of numbers into a gibberish string using the consonants for the first number and the vowels for the second.

{% code overflow="wrap" lineNumbers="true" %}

```rust
/// Decodes a gibbername into a blockchain location.
fn decode_gibbername(gname: &str) -> anyhow::Result<(BlockHeight, u32)> {
    let (height_num, index) = gibbercode::decode(&str)?;
    Ok((height_num.into(), index as u32))
}

/// Encodes a blockchain location into a gibbername.
fn encode_gibbername(height: BlockHeight, index: u32) -> String {
    // get the u64 out of the BlockHeight
    gibbercode::encode(height.0, index)
}
```

{% endcode %}

### Validating the start transaction

Once we have the blockchain location, we need to retrieve the start transaction. This can be done using `melprot`'s `Snapshot::get_transaction_by_posn()` function.

The start transaction should have a `data` field that says `"gibbername-v1"`, as well as one, and just one, output with denomination `Denom::NewToken`, and that output must have value `1`. This is the way we ensure that a given Gibbername is actually valid.

{% code overflow="wrap" lineNumbers="true" %}

```rust
async fn get_start_tx(client: &melprot::Client, gname: &str) -> anyhow::Result<(BlockHeight, TxHash)> {
    let (height, index) = decode_gibbername(gname)?;
    let snapshot = blockchain.snapshot(height);
    let start_tx = snapshot.get_transaction_by_posn(index).await?;
    // check the data field
    if start_tx.data != b"gibbername-v1" {
        anyhow::bail!("invalid data in start transaction");
    }
    // check the outputs
    let newtok_outputs: Vec<_> = start_tx
        .outputs
        .iter()
        .filter(|output| output.denom == Denom::NewToken)
        .collect();
    if newtok_outputs.len() == 1 && newtok_outputs[0].value == 1 {
        Ok((height, start_tx.hash()))
    } else {
        anyhow::bail!("invalid tx outputs");
    }
}

```

{% endcode %}

### Traversing the Catena chain

Finally, we can traverse the Catena chain to get the coin containing the final binding:

{% code overflow="wrap" lineNumbers="true" %}

```rust
async fn traverse_catena(
    client: &melprot::Client,
    start_height: BlockHeight,
    start_txhash: TxHash,
) -> anyhow::Result<CoinData> {
    // Traverse the graph in the forward direction using the `traverse_fwd` function. The closure passed to `traverse_fwd` defines the condition for picking the next transaction in the traversal
    // In this case, it returns the first (and unique!) output with either of the following conditions:
    //  - At the beginning find the denomination `Denom::NewToken`
    //  - Otherwise: find the custom denomination named after the starting transaction's hash
    let traversal: Vec<Transaction> = client
        .traverse_fwd(
            start_height,
            start_txhash,
            |txn: &Transaction| {
                txn.outputs
                    .iter()
                    .find(|cd| (txn.hash() == start_txhash && cd.denom == Denom::NewToken)
                        || cd.denom == Denom::Custom(start_txhash))
            },
        )
        .collect()
        .await?;

    // Get the last transaction in the traversal
    let last_transaction = traversal
        .last()
        .context("the traversal is empty?!")?;

    // Return an output with denomination `Denom::NewToken` if it exists
    // If not, the name is considered to be permanently deleted
    if let Some(final) = last_transaction
        .outputs
        .iter()
        .find(|cd| cd.denom == Denom::NewToken)
    {
        Ok(final)
    } else {
        // Catena chain stops because the custom token was destroyed.
        // this means the name is permanently deleted
        anyhow::bail!("name is permanently deleted")
    }
}
```

{% endcode %}

We can now easily build the gibbername lookup function!

{% code overflow="wrap" lineNumbers="true" %}

```rust
pub async fn lookup(
    client: &melprot::Client,
    gname: &str,
) -> anyhow::Result<Option<String>> {
    // find the coin containing the final binding
    let (start_height, start_txhash) = get_start_tx(client, gname).await?;
    let coin = traverse_catena(client, start_height, start_txhash).await?;
    // interpret the additional_data field as a UTF-8 string
    let binding = String::from_utf8_lossy(&coin.additional_data);
    Ok(Some(binding.to_owned()))
}
```

{% endcode %}

## Registering names

Registering names is a little different. Right of the bat we're faced with a problem: we need to _send_ a transaction into the blockchain rather than just reading existing data.

One possible way is to craft a transaction inside our library and send it by directly calling an RPC method on a full node (through something like `melprot::Client::raw_rpc()`). But this is hard, because we must somehow get hold of $MEL to pay transaction fees (possibly by asking the user to send money to some address?). Furthermore, even once we have $MEL, managing the money and the private keys securing it difficult, security-critical task.

Instead, we **ask the user's wallet to send a transaction for us**, and we simply wait until the user finishes doing so. In summary, here are the steps to register a new gibbername:

- Construct a _wallet URI_ using the standard `melwallet:` URI scheme, describing the transaction we need the user to send
- Prompt the user to "open" this URI with their wallet
  - Right now, we will prompt the user to do so manually.
  - Eventually, a graphical "Gibbername registrar app" should integrate with OS-specific URI-opening functionality.
- Wait for the transaction to commit and derive a gibbername from the location at which the transaction was committed

### Constructing the wallet URI

We begin by adding `melwallet_uri` to our Cargo dependencies:

```shell-session
$ cargo add melwallet_uri
```

We then write a helper function that generates a Gibbername registration transaction that registers the ownership of the name to a given address:

```rust
fn register_name_uri(address: Address, initial_binding: &str) -> melwallet_uri::MwUri {
    melwallet_uri::MwUriBuilder::new()
        .output(0, CoinData {
            denom: NewCoin::Denom,
            value: 1.into(),
            covhash: address,
            additional_data: initial_binding.as_bytes().into(),
        })
        .data(b"gibbername-v1")
        .build()
}
```

### Prompt and wait for the transaction

We can now write a function to send the transaction and wait for it to commit in the blockchain.

```rust
/// Prompts the user to register a gibbername, bound to the given initial binding, and blocks until the gibbername is registered, returning the gibbername.
pub async fn register(client: &melprot::Client, address: Address, initial_binding: &str) -> anyhow::Result<String> {
    let current_height = client.latest_snapshot().header().height;
    let uri = register_name_uri(address, initial_binding);
    println!("send with your wallet: {}", uri);
    // scan through all transactions happening on this address, starting at the block height right before we asked the user to send the transaction.
    let stream = client.stream_transactions_from(current_height);
    while let Some((transaction, height, posn)) = stream.next().await {
        if transaction.data == b"gibbername-v1" {
            return Ok(encode_gibbername(height, posn))
        }
    }
    unreachable!() // the stream is never supposed to terminate
}
```

We are now done with registering names! :rocket:

## Transferring names

Transferring names is left as an exercise to the reader.

{% hint style="info" %}

- Project setup
  - Make a library crate with melprot as a dep
- Lookups
  - Easiest part
  - Decode the gibbername as a block height and index
  - Use that to find the
- Registration
  - Need to send a transaction of a particular shape
  - Problem 1: need to prompt the user to send using wallet
    - Solution: wallet URL
  - Problem 2: need to know when the transaction has left the wallet
    - Solution: monitor for activity on the wallet
- Transfers
  - Just transfer the "NFT"
  - Similar approach to registration. Left to the reader (complete code on GitHub)
    {% endhint %}
