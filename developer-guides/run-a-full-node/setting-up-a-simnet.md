---
description: Learn how to run your own local "simnet" node.
---

# Setting up a local simnet

## Prerequisites

Make sure you have the required hardware and dependencies installed. Follow this [guide](melnode-quick-start.md) if you haven't already.

## Setting up a local simnet

For local development and testing, we can configure a local "simnet", or a fake network on our local computer.

The easiest way to do so is with our [`melsimnet`](https://github.com/mel-project/melnode/blob/master/src/bin/melsimnet.rs) binary.

```shell-session
cargo run --bin melsimnet -- create -s 1. -s 2. -s 3. -s 4.
```
Running this command will generate several (4 in this case) files and scripts to run the staker nodes in your custom local network. This means that staker 1 will have voting power equivalent to 1 SYM and staker 2 will have 2 SYM, etc.

Let's go through what this all means:

1. `run-staker-*.sh` - run an individual staker node
2. `run-all.sh` - run all of the staker nodes on a custom local network
3. `staker-*.yaml*` - defines the config for an individual staker node (explained below)
4. `genesis.yaml` - defines the genesis config for the custom local network (explained below)

After running some (or all) of the nodes, you'll be able to interact with the nodes via wallet client/daemon, melscan, even direct HTTP calls.

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
