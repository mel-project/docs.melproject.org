---
description: This is a brief, high-level introduction to Melmint
---

# Melmint Overview

## Introduction

Melmint's main job is to make sure `MEL`, our low-volatility base currency, is pegged to a reliable value. In order to be a useful store of value, `MEL` must not have a volatile price like Bitcoin or Ethereum.

Unlike existing conventional stablecoins, `MEL` does not rely on any external assets like fiats and oracles. Instead, Melmint is an oracle-free system that pegs the value of **1 MEL to 1 DOSC.**

At a high level, running an instance of Melminter lets people contribute information about "the current price of computation" to the network.&#x20;

### Why should I run melminter?

Running `melminter` (covered in the next section) provides the minter with a a reward of `ERG`, a Melmint-specific currency.

`ERG` itself is not a very useful currency on its own, so everyone would almost always want to sell it for something more valuable via [Melswap](../using-wallets/melswap-guide.md).

### Caveats

Minting `ERG` and converting into `MEL` is not always profitable. Because of the mechanics of Melmint, **minting is not** **guaranteed to be profitable** unless you either have a top-of-the-line CPU and cheap electricity, or Melmint is off-peg. This is because Melmint is not a proof-of-work consensus system, but rather a _pegging arbitrage_ system that is only really used to restore the `MEL/DOSC`peg.

Transient volatility in the `MEL/ERG` exchange rate may also affect profitability.

`melminter` makes no attempt at guessing whether or not minting is profitable.

`melminter` is a tool that provides a convenient, CLI interface for participating in Melmint by minting `ERG` and converting to `MEL`



## Further Reading

This was just a quick overview of Melmint. The complete guide on how MEL is stabilized, along with what happens under the hood can be found [here](../../concepts/sound-cryptoeconomics-with-truly-sound-money.md) in the conceptual documentation
