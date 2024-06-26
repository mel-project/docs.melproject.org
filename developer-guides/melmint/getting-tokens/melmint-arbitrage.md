---
description: How to use melminter to (hopefully) profit via arbitrage.
---

# Melmint arbitrage

## What is arbitrage?

Arbitrage is:

> The buying and selling of some asset (e.g. currency, securities, commodities, etc) in different markets to take advantage of differing prices for the same asset

In our case, we have multiple "markets", or liquidity pools, that contain asset pairs, such as MEL/SYM or MEL/ERG, and SYM/ERG. The Melmint mechanism [maintains the MEL/SYM pair](../../../concepts/melmint.md), but does not interact with the other asset pairs.&#x20;

In order to maintain the other pairs, we'll need YOU to do arbitrage and hopefully make some money.

<img src="../../../.gitbook/assets/image (1) (1).png" alt="" data-size="original">

## Using melwallet CLI to arbitrage

### Prerequisites

You'll need a wallet containing funds. [Here](../../using-wallets/getting-started.md) is a guide on how to set that up on the testnet.

### Time for some arbitrage :moneybag:

```shell-session
melwallet-cli --wallet-path <path to wallet> autoswap <value>  
```

Here, `<value>` is the amount of MEL  that you want to automatically swap for arbitrage.

This command will automatically execute trades on the "triangular" MEL/SYM/ERG pairs.

