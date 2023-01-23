---
description: 'FancyName: the trustless Themelio->Ethereum light-client relay'
---

# 🌉 FancyName guide

FancyName is a **Themelio/Ethereum relay contract**. It is essentially trustless and based on light-client verification on both sides.

FancyName enables the transfer of Themelio-native assets like MEL and SYM to Ethereum (or any other EVM chain) and back. This allows usage of Themelio assets in the Ethereum ecosystem --- for example, wMEL (wrapped MEL) can easily be plugged into the existing DeFi ecosystem as a novel low-volatility asset.

FancyName is 100% autonomous and decentralized. It's not a custodial bridge where you have to trust the bridge operators, nor do we rely on an external set of validators. Our relay leverages the native security of both underlying chains.

The functionality of the relay is simple:

* **Wrapping assets** from Themelio to Ethereum: Themelio assets (e.g. MEL) are locked up into a special covenant. A thin-client proof of the locking Themelio transaction is then submitted to an Ethereum smart contract, which then prints ERC-20 tokens (e.g. wMEL).
* **Unwrapping assets** like wMEL on Ethereum back to Themelio: ERC-20 wraped tokens are burnt with the Ethereum smart contract. A thin-client proof of the burn transaction is then used to unlock the locked assets on the Themelio side.

In the next section we will look at step-by-step instructions on how we can use FancyName to move our Themelio assets to the Ethereum network and after we will take a deeper look at FancyName's architecture and design.
