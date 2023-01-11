---
description: This is a brief, high-level introduction to Melmint
---

# Melmint Overview

## Introduction

Melmint's main job is to make sure `MEL`, our self-stabilizing base currency, is pegged to a reliable value. In order to be a useful store of value, `MEL` must not have a volatile price like Bitcoin or Ethereum.

Unlike existing conventional stablecoins, `MEL` does not rely on any external assets like fiats and oracles. Instead, Melmint is an oracle-free system that pegs the value of 1 `MEL` to 1 `DOSC`**.**

### **What is a DOSC?**

A “DOSC” is a “day of sequential computation”. It’s defined as the _cost of running a sequential computation for 24 hours, using the fastest processor available_.&#x20;

For example, a DOSC in the year 2000 is the cost of occupying the fastest single CPU core _available in 2000_ for 24 hours, while a DOSC in the year 2021 is the cost of doing the same with a 2021 processor.

### What makes DOSC a good peg target?

* It has a relatively stable purchasing power. Empirically, the “fastest processor” typically costs about the same, despite its performance drastically increasing over time. We explore this further in our [DOSC analysis data](https://github.com/themeliolabs/dosc-analysis).
* More importantly, it can be measured through a sequential proof-of-work on-chain by an autonomous mechanism with full endogenous trust.

## Participating in Melmint

At a high level, running an instance of Melminter lets people contribute information about "the current price of computation" to the network.

To participate in Melmint, you can [run your own](using-melminter.md) instance of `melminter`, a convenient CLI for minting `ERG` and converting to `MEL`.

### Further Reading

This was just a quick overview of Melmint. The complete guide on how `MEL` is stabilized, along with what happens under the hood, can be found [here](../../../concepts/sound-cryptoeconomics-with-truly-sound-money.md) in the conceptual documentation.
