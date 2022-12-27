---
description: This page describes how to mint MEL with Melminter
---

# Minting MEL with Melminter

## Introduction

`melminter` is a tool that provides a convenient, CLI interface for participating in Melmint by minting `ERG` and converting to `MEL`

## Installation and Setup

### Prerequisites

Ensure that you have:

- &#x20;An up-to-date `melminter` and `melwalletd` installed:

```shell-session
cargo install --locked melminter melwalletd
```

- A small amount of `MEL` in your wallet. (See setup instructions [here](../using-wallets/getting-started.md)). Melmint transactions are required to pay transaction fees, just like every other transaction.&#x20;
  - If you simply want to try this out on testnet, simply send some to yourself via a faucet transaction, as shown [here](../using-wallets/getting-started.md#fund-wallet).
  - If you wish to participate in mainnet Melminting, please ask for some `MEL` in the #beta-testers channel in our [Discord server](https://discord.gg/UXhxujHH).

## Starting the melminter

1. Start `melwalletd` as show [here](../using-wallets/getting-started.md#start-melwalletd).
2. Run the following:

```shell-session
melminter --payout <SOME_WALLET_ADDRESS>
```

The first time you run `melminter`, it will ask you to send a particular address a small amount of `MEL` in order to start. This is so that it can pay initial transaction fees. Use the `send` command [like so](../using-wallets/getting-started.md#send-funds).

TODO: add a good screenshot of the TUI

### Caveats

Minting `ERG` and converting into `MEL` is not always profitable. Because of the mechanics of Melmint, **minting is **_**probably not**_** profitable** unless you either have a top-of-the-line CPU and cheap electricity, or Melmint is off-peg. This is because Melmint is not a proof-of-work consensus system, but rather a _pegging arbitrage_ system that is only really used to restore the `MEL/DOSC`peg.

Transient volatility in the `MEL/ERG` exchange rate may also affect profitability.

`melminter` makes no attempt at guessing whether or not minting is profitable.

### Further Reading

This was just a simple example on how to use `melminter`

To understand what's happening under the hood, refer to this [document](https://app.gitbook.com/o/-LL705ARJ3zUDh-T0ICw/s/OYV1cKjV9tGHRgSj7gzr/).
