---
description: This page describes how to get started with staking on our  network.
---

# Staking basics

{% hint style="info" %}
This tutorial will be done on our testnet, so we can take advantage of printing MEL with faucet transactions.
{% endhint %}

## Installation and setup

### Create and fund a testnet wallet

First, we set up our wallets, following the instructions [here](../developer-guides/using-wallets/getting-started.md). Make sure to fund your wallet with a good amount of MEL (for transaction fees) and SYM (voting power)!&#x20;

### Install melnode

Next, you'll need to install `melnode`:

```shell-session
$ cargo install --locked melnode
```

We're almost ready to get started. Before we can start staking, we need to first run a staker node. There are two steps to do so:

1. Run a staker node
2. Give that staker node voting power

## Running a staker node

The first thing we'll need is a key pair, comprised of a public and private key for the staker node itself. The staker node will be identified by the public key, and the private key will be used internally while processing incoming stake transactions.

```shell-session
$ melnode --staker-wallet <payout-wallet-address>
```

The `--payout-addr` value is a mandatory wallet address that will receive the rewards for operating a staker node. Be sure to check that this is correct!

By default, `melnode` will auto-generate a new key pair for the staker node and save it at the default path of `$HOME/.melnode/staker-keys/`. The public and private keys will be saved as:

- Public key: `staker-key-ed25519.pub`
- Private key: `staker-key-ed25519`

Optionally, you can provide an existing key pair as a CLI argument (make sure the private key is stored somewhere safe). If the provided public key does not exist, `melnode` will autogenerate a new key pair, as mentioned above.

```shell-session
$ melnode --staker-pubkey <path-to-pubkey> --staker-private-key <path-to-private-key>
```

Once the node starts running, you should see something like this:

```shell-session
[2022-12-23T02:09:31Z INFO  melnode] staker melnode starting with pubkey <pubkey>
...
```

## Give voting power to the staker

Now that the staker node is up and running, it's time to give it some voting power so it can participate in the network's consensus!

{% hint style="danger" %}
If you are using this guide to stake on mainnet, please read through the [risks of staking](staking-risks.md) real SYM first!&#x20;
{% endhint %}

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ melwallet-cli stake &#x3C;amount-of-SYM> --to-stake &#x3C;staker-pubkey> --epochs 3
</strong>TRANSACTION RECIPIENTS
Address                                                 Amount          Additional data
t22272fg9r0k8k09qj06drzzjq9e0rw3asxfs1zrnaccwv5j6gq5tg  0.000100 MEL    ""
network fees                                            0.000254 MEL

STAKING EPOCH INFO
10000 SYM staking until: DATE (epoch N)
node 0xdeadbeefcafed00d given voting power from
  DATE (epoch N)
  to 
  DATE (epoch N+3)

WARNING: your SYM will be locked NOW! You might want to wait until you're closer to NEXT-EP-DATE before staking.
Proceed? (y/N) y

Are you sure? WARNING: THIS IS NOT REVERSIBLE. (y/N) y
</code></pre>

You will be prompted to confirm the transaction, fees, and epoch date range -- read this carefully before doing so! You should see an output like this afterwards:

```
Transaction hash: 5cc1de4ccbe52ef288377b8ad12b546223f48939892bba5592d9db2c21eabd77
Awaiting Confirmation... Confirmed at height 238!
Block Explorer: https://scan-testnet.Mel.org/blocks/238/5cc1de4ccbe52ef288377b8ad12b546223f48939892bba5592d9db2c21eabd77)
```

{% hint style="info" %}
Your staked funds will be **automatically** unstaked at the end of the specified staking window. There is no way to unstake them earlier, so be sure to double or triple check the stake duration!
{% endhint %}

## Receiving staking rewards

As a staker node operator, you are eligible for rewards, since you are actively contributing to our network's security and consensus.

Rewards are paid out to the `--staker-wallet` previously provided during the node startup process, and will be sent to the wallet address every block (\~30 seconds). Check the balance of the wallet to verify your rewards were successfully received:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ melwallet-cli summary -w bob
</strong>Wallet name:  bob (unlocked)
Network:      testnet
Address:      t04ncd9j314rt7jth5wmedz8j5tcz9w8cdcdk48ex9t2fkj44ekne0
Balance:      500.000000  MEL
               500.000000  MEL
Staked:       100.000000    SYM
</code></pre>

## Further reading

This was just a simple tutorial on how to run a staker node on the network. To learn more about the consensus process, continue [here](../concepts/consensus-and-staking/) for more details.
