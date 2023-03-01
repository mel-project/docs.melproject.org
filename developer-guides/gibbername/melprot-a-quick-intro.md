# melprot: a quick intro

## melprot::Client: a trustless thin client

The most basic tool for thin-client interaction in Mel is `melprot::Client`, a struct exposed by the melprot Rust crate that implements Mel's P2P protocol. `melprot::Client` is a thin client that can be used to query full nodes for information about blockchain contents.

But unlike a raw RPC client (which does exist as `melprot::NodeRpcClient`), `melprot::Client` internally validates Merkle-tree proofs and staker signatures so that it _avoids trusting any full node_. Everything that `melprot::Client` returns is backed by the decentralized, incentive-based trust of the Mel blockchain, and nothing can be faked by a malicious node or RPC network.

## Basic data model

```
a picture of the whole data model, looking roughly like:

[snapshot]  ... [snapshot]
     |
 [map]  [map] ...
```

### Snapshots

The data model of `melprot::Client` is largely focused on the **state snapshot**, which is an immutable, trustless view of _the state of the blockchain at a given height_. For instance, the following code obtains snapshots of the state at different heights, and queries how many unspent coins a particular address owns at the two heights.

```rust
// get a mainnet Client
let client = melprot::Client::autoconnect(NetID::Mainnet);
// get the current snapshot
let snap_current = client.latest_snapshot().await?;
// get the snapshot at block height 10000
let snap_10000 = client.snapshot(BlockHeight(10000)).await?;
// display how many UTXOs are labeled with foobar_address now vs at block 10000
println!("address {} has {} UTXOs now but {} UTXOs at block 10000",
   foobar_address,
   snap_current.coin_count(foobar_address).await?,
   snap_10000.coin_count(foobar_address).await?
);
```

### Looking up info

Within a snapshot at a given height, there are many mappings associated with the state of the blockchain at that given height and a variety of methods for conveniently looking them up. Some of the most important ones include:

* `get_coin(id: CoinID)`: given the ID (transaction hash and index) of a particular coin, return the coin data.
* `get_transaction(txhash: TxHash)`: given a transaction hash, return the `Transaction` if it exists.

### Moving a snapshot back in time

Since snapshots commit to the state of a blockchain at a particular height — which includes the previous history — they can be used to verify older claims about blockchain contents but not earlier claims. This is represented in `melprot` by `Snapshot::get_older(height: BlockHeight)`, which can move snapshots backwards in time, but not forwards:

```rust
// get a snapshot at block height 100
let snap_100 = client.snapshot(BlockHeight(100)).await?;
// get a snapshot at block height 50
let snap_50 = snap_100.get_older(BlockHeight(50)).await?;
// this fails:
let snap_200 = snap_100.get_older(BlockHeight(200)).await?;
```

## Coin graph traversal

A very common task for thin clients is traversing the **coin graph** of the blockchain, which is the global directed graph of all transactions spending and creating coins. For instance, this is a fragment of the coin graph centered around [a particular mainnet transaction in block 1901450](https://scan.themelio.org/blocks/1901450/674735b7b7e4163f7404715bd6b8433a8db523c52279ad07e2b4e88a6708d873):

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Given a known starting point — a particular transaction with a known location on the blockchain — the basic snapshot model detailed above allows easy traversal:

* **Moving backwards in time**: looking up the `CoinID` of a transaction input in a snapshot _older_ than the transaction itself retrieves a `CoinDataHeight` that contains the height in which the transaction input was committed to the blockchain. This gets you the location of a "parent" transaction.
* **Moving forwards in time**: to look up when an output is spent, a binary search can be done between the state in which the transaction was committed and the present, to see the exact block height at which the output was spent. Then, a snapshot at that height can be used to retrieve the "child" transaction that spent the output.

It's certainly possible to manually implement the above, but `melprot` provides to very convenient methods `Client::traverse_back` and `Client::traverse_fwd`. These functions take in a "starting" transaction (block height and transaction hash), as well as a closure to specify _which_ parent or child coin to follow to the next link, and return a `Stream` of `Transaction`s:

```rust
let client = melprot::Client::autoconnect(NetID::Mainnet);

let traversal = client.traverse_back(
   BlockHeight(1901450),
   "674735b7b7e4163f7404715bd6b8433a8db523c52279ad07e2b4e88a6708d873".parse()?,
   |tx| {
      // find the first input
      tx.outputs.get(0)
   }
).boxed();

while let Some(next) = traversal.next().await? {
   println!("transaction found: {:?}", next);
}
```

The above example will, starting from the [transaction mentioned previously](https://scan.themelio.org/blocks/1901450/674735b7b7e4163f7404715bd6b8433a8db523c52279ad07e2b4e88a6708d873), traverse its ancestry through the first input until it hits a transaction with no inputs (the first ever transaction in the blockchain!). Graphically, it essentially does this:

<figure><img src="../../.gitbook/assets/output (1).gif" alt=""><figcaption><p>Clicking on the first parent indefinitely</p></figcaption></figure>
