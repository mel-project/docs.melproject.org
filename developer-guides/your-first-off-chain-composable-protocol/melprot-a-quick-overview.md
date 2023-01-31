---
description: A quick overview of the melprot library.
---

# melprot: a quick overview

{% hint style="info" %}
Here we talk about the main data model of `melprot::Client`&#x20;

* Immutable "snapshots" of blockchain state
* Freely lookup SMT contents in the state
* Pervasive caching
* How UTXO graph exploration works
{% endhint %}

## Overview of melprot functionality

The main way to use `melprot` is through its thin, embeddable light client called `ValClient`. The client exposes a set of functionality for querying the blockchain.

Here's a brief example of how to instantiate the client in your Rust application:

```rust
let network = NetID::Mainnet;
let addr: SocketAddr = "bootstrap.melnet.org:12345".parse().unwrap();
let client = ValClient::connect_melnet2_tcp(network, addr).await?;
```

### Snapshots

`ValClient` lets you get a validated, immutable snapshot of the blockchain state at a given height. The snapshot lets you do things like:

* ask for all coins associated with a given covenant hash
* ask for a specific coin for a given coin ID
* get a block for a given transaction hash
* get a transaction for a given transaction hash
* freely lookup SMT contents in the state (TODO: elaborate?)
* get historical snapshots for older block heights

Here are a few examples of how to use snapshots:

```rust
let client = ValClient::connect_melnet2_tcp(network, addr).await?;
let snapshot = client.snapshot().await?;

// get an older snapshot
let older = snapshot.get_older(BlockHeight(100)).await?;

// get all coins with an associated covenant address
let coins_for_cov_address = snapshot.get_coins(covenant_hash).await?;

// get information about a stake for a given hash
let get_stake_info = snapshot.get_stake(stake_hash).await?;

// get a transaction for a given hash
let tx = snapshot.get_transaction(tx_hash).await?;

// get the raw, validated coin SMT for a given coin ID
let coin_smt = snapshot.get_smt_value(Substate::Coins, BlockHeight(2500)).await?;
```

### Get Trusted Stakers

`ValClient` also lets you get the entire set of trusted stakers. This is used during the height validation process.&#x20;

```rust
let staker_set = client.get_trusted_stakers().await?;
```



