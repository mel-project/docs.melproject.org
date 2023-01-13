---
description: 'FancyName: the trustless Themelio->Ethereum light-client relay'
---

# 🌉 FancyName guide

FancyName is a trustless, light-client relay that is natively verified and secured. It enables the transfer of Themelio coins to Ethereum and back, thus bootstrapping the Themelio ecosystem with all of the liquidity and defi potential available on the Ethereum network.

We heavily emphasize that the FancyName relay will be 100% trustless; it's not a custodial bridge where you have to trust the bridge operators, nor de we rely on an external set of validators. Our relay leverages the native security of both underlying chains.

The functionality of the relay is simple:

* to transfer coins from Themelio to Ethereum you first lock up the desired assets on the Themelio network and then provide proof of the locking transaction on the Ethereum side, where your coins will be minted as wrapped tokens
* to transfer your coins back to Themelio you would burn your tokens on Ethereum and then provide proof of the burn on the Themelio side, thus unlocking your coins

In the next section we will look at step-by-step instructions on how we can use FancyName to move our Themelio assets to the Ethereum network and after we will take a deeper look at FancyName's architecture and design.
