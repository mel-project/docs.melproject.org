---
description: A guide on how to use melminter to (hopefully) profit via arbitrage
---

# Melmint arbitrage

## What is arbitrage?

The high-level definition of arbitrage is:

> The buying and selling of some asset (e.g. currency, securities, commodities, etc) in different markets to take advantage of differing prices for the same asset

In our case, we have multiple "markets", or liquidity pools that contain asset pairs, such as `MEL/SYM` or `MEL/ERG`, and `SYM/ERG`.&#x20;

The Melmint mechanism described in the previous section maintains the `MEL/SYM` pair, but does not interact with the other asset pairs.&#x20;

In order to maintain the other pairs, we'll need YOU to make some :moneybag: through arbitrage.

<img src="../../../.gitbook/assets/image.png" alt="" data-size="original">

## Using melwallet CLI to autoswap

### Prerequisites

Make sure you have the following set up:

* A funded wallet. This is [a guide](../../../staking-guide/untitled.md) on how to set that up on testnet if needed

```shell-session
$ melwallet-cli autoswap <value> -w <wallet>  
```

Here, `<value>` should be the particular amount of a token that you want to swap.

This command will automatically execute trades on the "triangular" `MEL/SYM/other` pairs.

TODO: explain how to check the results, etc.
