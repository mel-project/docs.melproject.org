---
description: This page describes how to use Melminter
---

# Using Melminter

## Introduction

`melminter` is a tool that provides a convenient, CLI interface for participating in Melmint by minting `ERG` and converting to `MEL`

## Installation and Setup

### Prerequisites

Ensure that you have an up-to-date version of `melminter` installed:

```shell-session
cargo install --locked melminter
```

* A small amount of `MEL` in your wallet. (See setup instructions [here](../using-wallets/getting-started.md)). Melmint transactions are required to pay transaction fees, just like every other transaction.
  * If you simply want to try this out on testnet, simply send some to yourself via a faucet transaction, as shown [here](../using-wallets/getting-started.md#fund-wallet).
  * If you wish to participate in mainnet Melminting, please ask for some `MEL` in the #beta-testers channel in our [Discord server](https://discord.gg/UXhxujHH).

## Running the melminter

```shell-session
$ melminter --payout <SOME_WALLET_ADDRESS>
```

The first time you run `melminter`, it will ask you to send a particular address a small amount of `MEL` in order to start. This is so that it can pay initial transaction fees. Use the `send` command [like so](../using-wallets/getting-started.md#send-funds).&#x20;

Running `melminter` will reward you with ERGs, which can be swapped for other currencies like MEL, SYM, or custom tokens via [Melswap](../using-wallets/melswap-guide.md).

