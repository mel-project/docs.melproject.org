# melprot: a quick intro

## melprot::Client: a trustless thin client

The most basic tool for thin-client interaction in Mel is `melprot::Client`, a struct exposed by the melprot Rust crate that implements Mel's P2P protocol. `melprot::Client` is a thin client that can be used to query full nodes for information about blockchain contents.

But unlike a raw RPC client (which does exist as `melprot::NodeRpcClient`), `melprot::Client` internally validates Merkle-tree proofs and staker signatures so that it _avoids trusting any full node_. Everything that `melprot::Client` returns is backed by the decentralized, incentive-based trust of the Mel blockchain, and nothing can be faked by a malicious node or RPC network.

## Basic data model

### Snapshots

The data model of `melprot::Client` is largely focused on the **state snapshot**, which encapsulates _the state of the blockchain at a given height_. For instance, the following code obtains snapshots of the state at different heights, and query

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

\
