---
description: This page describes how to run your own local "simnet" node
---

# Setting up a local simnet

## Prerequisites

Make sure you have the required hardware and dependencies installed. Follow this [guide](melnode-quick-start.md) if you haven't already.

## Setting up a local simnet

For local development and testing, we can configure a local "simnet", or a fake network on our local computer.

To do so, we will need to use the `melnode` command with a combination of these three options

* `--bootstrap <bootstrap-address>`
* `--override genesis <path-to-genesis-config>`
* `--staker-cfg <path-to-staker-config>`

```shell-session
$ melnode --bootstrap <bootstrap-ip-address> --override-genesis <path-to-genesis-config> staker-cfg <path-to-staker-config>
```

### Bootstrap address

This is an IP address that specifies the bootstrap address for our local node. This lets us bootstrap only with ourselves instead of with a remote node on test/mainnet.

### Custom Genesis Configuration

To start our own custom network, `melnode` accepts a YAML config file similar to the following:

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
# we need to specify ourselves in order to produce any blocks; "themelio-crypttool generate-ed25519" (install via cargo) can generate a keypair for us
stakes:
  deadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef:
    pubkey: 4ce983d241f1d40b0e5b65e0bd1a6877a35acaec5182f110810f1276103c829e
    e_start: 0
    e_post_end: 100000 # essentially never end the stake
    syms_staked: 10000 # does not matter
# Initial fee pool
init_fee_pool: 10000
```

### Custom Staker Configuration

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

