# melprot: a quick intro

## melprot::Client: a trustless thin client

The most basic tool for thin-client interaction in Mel is `melprot::Client`, a struct exposed by the melprot Rust crate that implements Mel's P2P protocol. `melprot::Client` is a thin client that can be used to query full nodes for information about blockchain contents.

But unlike a raw RPC client (which does exist as `melprot::NodeRpcClient`), `melprot::Client` internally validates Merkle-tree proofs and staker signatures so that it _avoids trusting any full node_. Everything that `melprot::Client` returns is backed by the decentralized, incentive-based trust of the Mel blockchain, and nothing can be faked by a malicious node or RPC network.

## Basic data model

```
a picture of the whole data model.

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

- `get_coin(id: CoinID)`: given the ID (txhash and index) of a particular coin, return the coin data.

**TODO: FILL THIS IN WITH THE MOST COMMON ONES**

### Moving a snapshot back in time

Since snapshots commit to the state of a blockchain at a particular height &mdash; which includes the previous history &mdash; they can be used to verify older claims about blockchain contents but not earlier claims. This is represented in `melprot` by `Snapshot::get_older(height: BlockHeight)`, which can move snapshots backwards in time, but not forwards:

```rust
// get a snapshot at block height 100
let snap_100 = client.snapshot(BlockHeight(100)).await?;
// get a snapshot at block height 50
let snap_50 = snap_100.get_older(BlockHeight(50)).await?;
// this fails:
let snap_200 = snap_100.get_older(BlockHeight(200)).await?;
```

## Coin graph traversal

A very common task for thin clients is traversing the graph of coins, which is the directed graph of transactions spending and creating coins. For instance
