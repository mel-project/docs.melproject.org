---
description: This page shows how to participate in Melmint with the melminter CLI
---

# Using Melminter

## Installation and Setup

Ensure that you have an up-to-date version of `melminter` installed:

```shell-session
cargo install --locked melminter
```

You will need:

* A small amount of `MEL` in your wallet. (See setup instructions [here](../../../using-wallets/getting-started.md)). This is because Melmint transactions are required to pay transaction fees, just like every other transaction.

{% hint style="info" %}
If you simply want to try this out on the testnet, you can acquire testnet `MEL` yourself via a faucet transaction, as shown [here](../../../using-wallets/getting-started.md#fund-wallet).

If you wish to participate in mainnet Melminting, please ask for some `MEL` in the #beta-testers channel in our [Discord server](https://discord.gg/UXhxujHH).
{% endhint %}

## Running  melminter

```shell-session
$ melminter --payout <SOME_WALLET_ADDRESS>
```

The first time you run `melminter`, it will ask you to send a particular address a small amount of `MEL` in order to start. This is so that it can pay initial transaction fees. Use the `send` command:

```shell-session
$ melwallet-cli unlock -w <wallet>
$ melwallet-cli send -w <wallet> --to <address>, <amount>
```

Afterwards, you should see a TUI like this:

<img src="../../../../.gitbook/assets/image.png" alt="" data-size="original">

Running `melminter` will reward you with ERGs, which can be swapped for other currencies like MEL, SYM, or custom tokens via [Melswap](../../../using-wallets/melswap-guide.md). Note the _daily return_ line in the terminal output, which predicts how much computational work (in DOSC) the minter will do in 24 hours, as well as how much MEL that will generate.

## Caveats

Minting `ERG` and converting to `MEL` is not always profitable. Because of the mechanics of Melmint, **minting is not** **guaranteed to be profitable** unless you either have a top-of-the-line CPU and cheap electricity, or Melmint is off-peg. This is because Melmint is not a proof-of-work consensus system, but rather a _pegging arbitrage_ system that is only really used to restore the `MEL/DOSC`peg.

Transient volatility in the `MEL/ERG` exchange rate may also affect profitability.

melminter makes no attempt at guessing whether or not minting is profitable.

melminter is a tool that provides a convenient, CLI interface for participating in Melmint by minting `ERG` and converting to `MEL`.
