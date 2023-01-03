---
description: >-
  A brief overview of Melmint, the main mechanism used to issue and stabilize
  MEL
---

# Melmint overview

## Overview

Melmint's main job is to make sure `MEL`, our base currency and stablecoin, is pegged to a reliable value. In order to be a useful store of value, `MEL` must not have a volatile price like Bitcoin or Ethereum.

Unlike existing conventional stablecoins, `MEL` does not rely on any external assets like fiats and oracles. Instead, Melmint is an oracle-free system that pegs the value of **1 MEL to 1 DOSC.**

### What is a DOSC?

A “DOSC” is a “day of sequential computation”. It’s defined as the _cost of running a sequential computation for 24 hours, using the fastest processor available_.&#x20;

For example, a DOSC in the year 2000 is the cost of occupying the fastest single CPU core _available in 2000_ for 24 hours, while a DOSC in the year 2021 is the cost of doing the same with a 2021 processor.

### What makes DOSC a good peg target?

* It has a relatively stable purchasing power. Empirically, the “fastest processor” typically costs about the same, despite its performance drastically increasing over time. We explore this further in our [DOSC analysis data](https://github.com/themeliolabs/dosc-analysis).
* More importantly, it can be measured through a "sequential proof-of-work" on-chain by an autonomous mechanism with full endogenous trust.

## Melminter

At a high level, running an instance of Melminter lets people contribute information about "the current price of computation" to the network.&#x20;

### Why should I run melminter?

Running `melminter` (covered in the next section) provides the minter with a a reward of `ERG`, a Melmint-specific currency with the value of `k ERGs = 1 DOSC`.&#x20;

`ERG` itself is not a very useful currency on its own, so almost everyone would almost always want to sell it for something more valuable via [Melswap](../using-wallets/melswap-guide.md).

### Caveats

Minting `ERG` and converting into `MEL` is not always profitable. Because of the mechanics of Melmint, **minting is probably not** **profitable** unless you either have a top-of-the-line CPU and cheap electricity, or Melmint is off-peg. This is because Melmint is not a proof-of-work consensus system, but rather a _pegging arbitrage_ system that is only really used to restore the `MEL/DOSC`peg.

Transient volatility in the `MEL/ERG` exchange rate may also affect profitability.

`melminter` makes no attempt at guessing whether or not minting is profitable.



## Further Reading

This was just a quick overview of Melmint. The complete guide on how MEL is stabilized, along with what happens under the hood can be found [here](../../concepts/sound-cryptoeconomics-with-truly-sound-money.md) in the conceptual documentation.



