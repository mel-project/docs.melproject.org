---
description: Learn how to run your own local "simnet" node.
---

# Setting up a local simnet

## Prerequisites

Make sure you have the required hardware and dependencies installed. Follow this [guide](melnode-quick-start.md) if you haven't already.

## Setting up a local simnet

For local development and testing, we can configure a local "simnet", or a fake network on our local computer.

The easiest way to do so is with our [`melsimnet`](https://github.com/mel-project/melnode/blob/master/src/bin/melsimnet.rs) binary. In the `melnode` directory:

```shell-session
cargo run --bin melsimnet -- create -s 1. -s 2. -s 3. -s 4.
```
Running this command will generate several (4 in this case) files and scripts to run the staker nodes in your custom local network. This means that staker 1 will have voting power equivalent to 1 SYM and staker 2 will have 2 SYM, etc.

Let's go through what this all means:

1. `run-staker-*.sh` - run an individual staker node
2. `run-all.sh` - run all of the staker nodes on a custom local network
3. `staker-*.yaml*` - defines the config for an individual staker node (explained below)
4. `genesis.yaml` - defines the genesis config for the custom local network (explained below)

After running some (or all) of the nodes, you'll be able to interact with the nodes via `melwallet-client`, `melminter`, or even direct HTTP calls.

```shell-session
./run-all.sh
```
Should produce logs like:
```
[2024-05-11T14:28:09Z INFO  melnode] melnode v0.20.7 initializing...
[2024-05-11T14:28:09Z DEBUG melnode::storage::storage] about to sqlite
[2024-05-11T14:28:09Z DEBUG melnode::storage::storage] sqlite initted
[2024-05-11T14:28:09Z DEBUG melnode::storage::storage] about to mesha
[2024-05-11T14:28:09Z DEBUG melnode::args] node storage opened
[2024-05-11T14:28:09Z INFO  melnode] bootstrapping with [127.0.0.1:2000]
[2024-05-11T14:28:09Z DEBUG melnode::node] starting to listen at 127.0.0.1:2000
[2024-05-11T14:28:19Z DEBUG melnode::staker] starting consensus for 1...
[2024-05-11T14:28:19Z WARN  melnode::staker] mempool not at the right height, trying again
[2024-05-11T14:28:29Z DEBUG melnode::staker] starting consensus for 1...
[2024-05-11T14:28:29Z DEBUG melstf::state] changing fee multiplier 100 by 1
[2024-05-11T14:28:29Z DEBUG melnode::staker] proposed state has 0 transactions
[2024-05-11T14:28:32Z DEBUG melnode::staker] 1/127.0.0.1:5000 DECIDED on a block with 240 bytes within 3.155822014s
[2024-05-11T14:28:32Z DEBUG melstf::state] applied a batch of 0 txx to #<c99d36b369d0fc1bd494b84db635c5964e8359fafff00a1732d19090c3595e41> => #<c99d36b369d0fc1bd494b84db635c5964e8359fafff00a1732d19090c3595e41>
[2024-05-11T14:28:32Z DEBUG melstf::state] changing fee multiplier 100 by 1
[2024-05-11T14:28:32Z DEBUG melnode::storage::storage] applied block 1 / 64e3f33aa793f118ef09fa8aa72a7f6b7b5831dba1f8052d8ac1506e04883a09 in 5.01ms (history insertion 2.42ms)
[2024-05-11T14:28:32Z DEBUG melnode::staker] 1/127.0.0.1:5000 COMMITTED the newly decided block within 3.164806678s
```

### melwallet-cli, melminter
To use melwallet or melminter on your local simnet, first set the `MELBOOTSTRAP` environment variable to a hard-coded block height and header hash to bootstrap clients on this network:
```
export MELBOOTSTRAP=<network-name>:<block-height>:<header-hash>
```
You can find all the information needed from the node logs. From the logs above, you can set:
```
export MELBOOTSTRAP=custom02:1:64e3f33aa793f118ef09fa8aa72a7f6b7b5831dba1f8052d8ac1506e04883a09
```

To use the wallet, run `melwallet-cli` with the `--bootstrap` flag to specify the socket address for connecting to the local node:
```
melwallet-cli --wallet-path my-wallet.json --bootstrap 127.0.0.1:2000 create --network custom02
```

Similarly with `melminter`:
```
melminter --bootstrap 127.0.0.1:2000 --payout <wallet-address>
```

Local simnets all support faucet transactions:
```
melwallet-cli --wallet-path my-wallet.json --bootstrap 127.0.0.1:2000 send-faucet --wait
```

### Custom genesis configuration

This is only needed to start our own custom network, `melnode` accepts a YAML config file similar to the following:

```yaml
network: custom02 # anything from custom02..custom08
# specifies the "initial stash" of money in the genesis block
init_coindata:
  # what address gets the initial supply of money
  covhash: t5xw3qvzvfezkb748d3zt929zkbt7szgt6jr3zfxxnewj1rtajpjx0
  # how many units (in millionths)
  value: 1000000
  # denomination
  denom: MEL
  # additional data in the UTXO, as a hex string
  additional_data: ""
# specifies all the stakers with consensus power.
# we need to specify ourselves in order to produce any blocks; "Mel-crypttool generate-ed25519" (install via cargo) can generate a keypair for us
stakes:
  deadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef:
    pubkey: 4ce983d241f1d40b0e5b65e0bd1a6877a35acaec5182f110810f1276103c829e
    e_start: 0
    e_post_end: 100000 # essentially never end the stake
    syms_staked: 10000 # does not matter
# Initial fee pool
init_fee_pool: 10000
```

### Custom staker configuration

If we want to run a local custom staker node, `melnode` accepts a YAML config file:

```yaml
# secret key; must correspond to "stakes.dead[...]beef.pubkey" in the network config
signing_secret: 5b4c8873cbdb089439d025e9fa817b1df1128231699131c245c0027be880d4d44ce983d241f1d40b0e5b65e0bd1a6877a35acaec5182f110810f1276103c829e
# address for staker-network communication, this can be arbitrary
listen: 127.0.0.1:20000
# must be same as "listen"
bootstrap: 127.0.0.1:20000
# where block rewards are sent
payout_addr: t5xw3qvzvfezkb748d3zt929zkbt7szgt6jr3zfxxnewj1rtajpjx0
# vote for this fee multiplier (higher values charge more fees)
target_fee_multiplier: 10000
```