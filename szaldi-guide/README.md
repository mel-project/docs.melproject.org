---
description: 'Szaldi: the trustless Mel->Ethereum light-client relay.'
---

# 🌉 Szaldi guide

{% hint style="warning" %}
Szaldi is under heavy development and **not** yet ready for public use.

Both Szaldi and these docs are WIP and **following them will not work right now.**
{% endhint %}



Szaldi is a **Mel/Ethereum relay contract**. It is essentially trustless and based on light-client verification on both sides.

Szaldi enables the transfer of Mel-native assets like MEL and SYM to Ethereum (or any other EVM chain) and back. This allows usage of Mel assets in the Ethereum ecosystem — for example, wMEL (wrapped MEL) can easily be plugged into the existing DeFi ecosystem as a novel, low-volatility asset.

Szaldi is 100% autonomous and decentralized. It's not a custodial bridge where you have to trust the bridge operators, nor do we rely on an external set of validators. Our relay leverages the native security of both underlying chains.

The functionality of the relay is simple:

* **Wrapping assets** from Mel to Ethereum: Mel assets (e.g. MEL) are locked up into a special covenant. A light-client proof of the locking Mel transaction is then submitted to an Ethereum smart contract, which then prints ERC-1155 tokens (e.g. wMEL).
* **Unwrapping assets** like wMEL on Ethereum back to Mel: ERC-1155 wrapped tokens are burnt with the Ethereum smart contract. A light-client proof of the burn transaction is then used to unlock the locked assets on the Mel side.

In the next section we will look at step-by-step instructions on how we can use Szaldi to move our Mel assets to the Ethereum network and after we will take a deeper look at Szaldi's architecture and design.
