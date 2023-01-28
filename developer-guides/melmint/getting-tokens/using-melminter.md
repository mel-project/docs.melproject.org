---
description: This page shows how to participate in Melmint with the melminter CLI.
---

# Using melminter

## Installation and Setup

Ensure that you have an up-to-date version of `melminter` installed:

```shell-session
$ cargo install --locked melminter
```

You will need:

* A small amount of MEL in your wallet (see setup instructions [here](../../using-wallets/getting-started.md)). This is because Melmint transactions are required to pay transaction fees, just like every other transaction.

{% hint style="info" %}
If you simply want to try this out on the testnet, you can acquire testnet MEL yourself via a faucet transaction, as shown [here](../../using-wallets/getting-started.md#fund-wallet).

If you wish to participate in mainnet melminting, please ask for some MEL in the #beta-testers channel in our [Discord server](https://discord.gg/UXhxujHH).
{% endhint %}

## Running  melminter

```shell-session
$ melminter --payout <payout-wallet-address>
```

The first time you run `melminter`, it will ask you to send a particular address a small amount of MEL in order to start. This is so that it can pay initial transaction fees. Simply paste the `melwallet:`  URL given to you by `melminter`:

```shell-session
$ melwallet-cli unlock -w <wallet>
$ melwallet-cli open-url -w <wallet> <url-from-melminter>
```

Afterwards, you should see a nice TUI like this:

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Running `melminter` will generate ERG, a temporary token representing computation, and immediately exchange it for MEL. Note the _daily return_ line in the terminal output, which predicts how much computational work (in DOSC) the minter will do in 24 hours, as well as how much MEL that will buy.

## Caveats

Bidding for MEL with computation is not always profitable. Because of the mechanics of Melmint, **minting is very unlikely to be profitable** unless you either have a top-of-the-line CPU and cheap electricity, or Melmint is off-peg. This is because Melmint is not a proof-of-work consensus system, but rather a _pegging arbitrage_ system that is only really used to restore the MEL/DOSC .

Transient volatility in the MEL/ERG exchange rate may also affect profitability.

`melminter` makes no attempt at guessing whether or not minting is profitable.
