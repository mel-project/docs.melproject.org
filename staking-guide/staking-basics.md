---
description: This page describes how to get started with staking on our  network
---

# Staking Basics

{% hint style="info" %}
This tutorial will be done on our testnet, so we can take advantage of printing MEL with faucet transactions.
{% endhint %}

## Installation and prerequisites

### Create and fund a testnet wallet

First, we set up our wallets, following the instructions [here](../developer-guides/using-wallets/getting-started.md), starting from the beginning until you've sent yourself some MEL via a faucet transaction on the testnet, which will be used to pay transaction fees.

### Fund the wallet with SYM as well

We can acquire some SYM by swapping some of our MEL. Follow the guide [here](../developer-guides/using-wallets/melswap-guide.md#swapping-coins) and get some SYM into your wallet.

### Install melnode

Next, you'll need to install `melnode`:

```
$ cargo install --locked melnode
```

We're almost ready to get started. Before we can start staking, we need to first run a staker node. There are two steps to do so:

1. Run a staker node
2. Give that staker node voting power

## Running a staker node

The first thing we'll need is a key-pair, comprised of a public and private key for the staker node itself. The staker node will be identified by the public key, and the private key will be used internally while processing incoming stake transactions.

```
$ melnode --payout-addr <payout-wallet-address>
```

The `--payout-addr` value is a mandatory wallet address that will receive the rewards for operating a staker node. Be sure to check that this is correct!

By default, `melnode` will auto-generate a new keypair for the staker node and save them at a default path of `$HOME/.melnode/staker-keys/`. The public and private keys will be saved as:

* Public key: `staker-key-ed25519.pub`
* Private key: `staker-key-ed25519`

Optionally, you can provide an existing keypair as a CLI argument (make sure the private key is stored somewhere safe!) If the provided public key does not exist, `melnode` will autogenerate a new keypair as mentioned above.

```
$ melnode --staker-pubkey <path-to-pubkey> --staker-private-key <path-to-private-key>
```

Once the node starts running, you should see something like this:

```
[2022-12-23T02:09:31Z INFO  melnode] staker melnode starting with pubkey <pubkey>
...
```

## Give voting power to the staker

Now that the staker node is up and running, it's time to give it some voting power so it can participate in the network's consensus!

<pre class="language-shell-session"><code class="lang-shell-session">$ melwallet-cli stake &#x3C;amount-of-SYM> --to-stake &#x3C;staker-pubkey> --epochs 3
<strong>
</strong>TRANSACTION RECIPIENTS
Address                                                 Amount          Additional data
t22272fg9r0k8k09qj06drzzjq9e0rw3asxfs1zrnaccwv5j6gq5tg  0.000100 MEL    ""
network fees                                            0.000254 MEL

STAKING EPOCH INFO  
10000 SYM staking until: DATE (epoch N)
node 0xdeadbeefcafed00d given voting power from
  DATE (epoch N)
  to 
  DATE (epoch N)

WARNING: your SYM will be locked NOW! You might want to wait until you're closer to NEXT-EP-DATE before staking.
Proceed? (y/N) y

Are you sure? WARNING: THIS IS NOT REVERSIBLE. (y/N) y
</code></pre>

You will be prompted to confirm the transaction, fees, and epoch date range -- read this carefully before doing so! You should see an output like this afterwards:

```
Transaction hash: 5cc1de4ccbe52ef288377b8ad12b546223f48939892bba5592d9db2c21eabd77
Awaiting Confirmation... Confirmed at height 238!
Block Explorer: https://scan-testnet.themelio.org/blocks/238/5cc1de4ccbe52ef288377b8ad12b546223f48939892bba5592d9db2c21eabd77)
```

## Receiving staking rewards

As a staker node operator, you are eligible for rewards, since you are actively contributing to our network's security and consensus.&#x20;

Rewards are paid out to the `--payout-addr` previously provided during the node startup process, and will be sent to the wallet address every block (\~30 seconds). Check the balance of the wallet to verify your rewards were successfully received:

```shell-session
$ melwallet-cli summary -w bob
Wallet name:  bob (unlocked)
Network:      testnet
Address:      t04ncd9j314rt7jth5wmedz8j5tcz9w8cdcdk48ex9t2fkj44ekne0
Balance:      500.000000  MEL
               500.000000  MEL
Staked:       100.000000    SYM
```



## Futher Reading

This was just a simple tutorial on how to run a staker node on the network. To learn more about the consensus process, continue here (TODO) for more.
