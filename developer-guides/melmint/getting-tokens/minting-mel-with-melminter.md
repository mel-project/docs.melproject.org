---
description: This is a brief, high-level introduction to Melmint.
---

# Melmint overview

## Introduction

Melmint is the our key mechanism for stabilizing MEL, Mel's base currency. MEL avoids the volatile price of traditional cryptocurrencies like BTC and ETH, making it a much better store of value and unit of account.

But, unlike "stablecoins", MEL does not rely on any external trust in fiat assets or oracles. Instead, Melmint is an oracle-free system that keeps the value of 1 MEL around **1 DOSC**.

### **What is a DOSC?**

A “DOSC” is a “day of sequential computation”. It’s defined as the _cost of running a sequential computation for 24 hours, using the fastest processor available_.&#x20;

For example, a DOSC in the year 2000 is the cost of occupying the fastest single CPU core _available in 2000_ for 24 hours, while a DOSC in the year 2021 is the cost of doing the same with a 2021 processor.

### What makes DOSC a good peg target?

- It has a relatively stable purchasing power. Empirically, the “fastest processor” typically costs about the same, despite its performance drastically increasing over time. We explore this further in our [DOSC analysis data](https://github.com/Mellabs/dosc-analysis). There are also deeper _a priori_ reasons why [processor time is a good long-term measure of value](https://forum.Mel.org/t/some-thoughts-on-melmint-stability/29).
- More importantly, it can be measured through a sequential proof-of-work on-chain by an autonomous mechanism with full endogenous trust. No oracle needs to be trusted to tell us how much a DOSC is.

## Participating in Melmint

### Contribute CPU measurements

To participate in Melmint, you can [run your own](using-melminter.md) instance of `melminter`, a convenient CLI that turns CPU computation into MEL.

Though it turns computation into money, `melminter` is **not** a "miner" and does not help secure the network. Instead, it _bids_ for MEL \__ using \_computation_, contributing information about the current price of computation to the network. This then drives the Melmint mechanism to keep the 1 MEL = 1 DOSC peg.

### Make arbitrage trades

Melmint's stability depends on a community of traders profiting from arbitrage between the MEL/SYM, ERG/SYM, and ERG/MEL Melswap pools. Learn more in the [article on Melmint arbitrage](melmint-arbitrage.md)! &#x20;

## Further Reading

This was just a quick overview of Melmint. The complete guide on how MEL is stabilized, along with what happens under the hood, can be found [here](../../../concepts/sound-cryptoeconomics-with-truly-sound-money.md) in the conceptual documentation.
