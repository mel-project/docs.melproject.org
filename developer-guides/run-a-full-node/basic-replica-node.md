---
description: Running a replica node with melnode.
---

# Basic replica node

**Replica nodes** simply validate and replicate blocks that the network has already produced. They are the most common type of full node, and running one does not require staking anything.

## On the mainnet

### Starting melnode

Running the `melnode` command without any arguments starts an instance running on the mainnet. 

```shell-session
melnode
```

You will see output looking like this:

```shell-session
[2022-12-12T16:40:48Z INFO  melnode] melnode v0.13.2 initializing...
[2022-12-12T16:40:48Z DEBUG melnode::args] database opened at "/home/user/.melnode/"
[2022-12-12T16:40:48Z INFO  melnode::storage::storage] HIGHEST AT 0
[2022-12-12T16:40:48Z DEBUG melnode::args] node storage opened
[2022-12-12T16:40:48Z INFO  melnode] bootstrapping with [185.177.126.98:41814]
[2022-12-12T16:40:48Z DEBUG melnode::protocols::node] starting to listen at 0.0.0.0:41814
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 1 of length 215 in 1.08ms (insert 9.54ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 2 of length 215 in 0.51ms (insert 0.26ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 3 of length 215 in 0.43ms (insert 0.21ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 4 of length 215 in 0.45ms (insert 0.22ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 5 of length 215 in 0.43ms (insert 0.24ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 6 of length 215 in 0.44ms (insert 0.23ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 7 of length 215 in 0.44ms (insert 0.24ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 8 of length 215 in 0.46ms (insert 0.18ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 9 of length 215 in 0.46ms (insert 0.15ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 10 of length 215 in 0.46ms (insert 0.16ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 11 of length 215 in 0.46ms (insert 0.16ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 12 of length 215 in 0.44ms (insert 0.25ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 13 of length 215 in 0.46ms (insert 0.20ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 14 of length 215 in 0.64ms (insert 0.25ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 15 of length 215 in 0.46ms (insert 0.22ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 16 of length 215 in 0.47ms (insert 0.13ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 17 of length 215 in 0.48ms (insert 0.13ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 18 of length 215 in 0.47ms (insert 0.23ms)
[2022-12-12T16:40:48Z DEBUG melnode::storage::storage] applied block 19 of length 215 in 0.48ms (insert 0.13ms)
```

{% hint style="warning" %}
There is an optional flag `--index-coins` that is recommended. Certain coin-related RPCs will be disabled if this flag is not set. Do note that the indexer will take up extra memory.
{% endhint %}

It will take _quite_ a long time to synchronize all the blocks from the network (usually 10+ hours as of April 2024).

### Participating in peering

{% hint style="warning" %}
To participate in peering, you must have a publicly reachable IP address. Most home internet setups do not give you a public IPv4 address!
{% endhint %}

By default, `melnode` doesn't do much other than downloading blocks. The default listening port is `localhost:41814`: since this is on localhost, no other computers can connect to the node.

To actually contribute to block propagation on the network, you need to expose `melnode` to other networks:

```shell-session
melnode --listen [::1]:41814 --advertise auto
```

We add two flags:

* `--listen [::1]:41814` listens on port 41814 on all network interfaces
* `--advertise auto` automatically guesses our public IP address for incoming connections on the P2P network

If this works, you should soon see output like:

```shell-session
[2022-12-12T16:40:48Z DEBUG melnode::network] incoming connection from 100.64.3.2!
```

indicating that you are helping other nodes connect to the network.

## On the testnet

### Basic functionality

To replicate testnet blocks, add `--network testnet` to any `melnode` command:

```shell-session
melnode --network testnet
```

### Faucets and staking

A distinct feature of the testnet is that **faucets** can be used to generate "free money", which can then be used to test consensus and staking (future feature).
