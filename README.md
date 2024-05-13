---
description: "A post-blockchain  Web3"
---

# A post-blockchain Web3

## "Web3" is trapped in itself

The web3/crypto community has an incredibly powerful vision of secure user autonomy and coordination. And it's not without foundation --- blockchains and web3 protocols have a combination of _unique superpowers_ that no previous network protocol achieve: censorship resistance, decentralized trust, and user-aligned incentives.

But we don't actually see a true "Web3": a successor to Web2 with radically better user autonomy and coordination. The current crypto world is viciously meta: with the notable exception of collectibles like NFTs and memecoins, most crypto apps are about speculating on tokenized shares of crypto apps. DeFi, for example, mostly finances speculation on tokens (many of which belong to DeFi projects!), rather than anything outside the crypto sphere. It's as absurd as if the primary use of web2 tech was to host brokerages where people can speculate on web2 tech company stocks.

This problem is usually posed as "mainstream crypto adoption", and the usual solution everyone peddles is some combination of "better UX" and shoehorning external systems into the self-referential crypto ecosystem (like "real-world assets"). But we've tried this for the better half of a decade, and it's not working.

## Mel: freeing crypto superpowers from crypto

Mel is a blockchain engineered for a Web3 paradigm shift.

Instead of smart-contract dApps running on a world computer, we envision what we call a **post-blockchain Web3** –- an internet-scale ecosystem with full-stack decentralized security. Mel is the embedded security component for this ecosystem, _not an app platform_.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

This means a largely non-blockchain world with two key properties:

1. **Off-chain programs trustlessly compose with on-chain programs.** Off-chain programs, whether dApp frontends or whole alt-tech stacks, must integrate on-chain functionality without sacrificing L1 security.&#x20;

For instance, a frontend for an anonymously published blog can't be censorable by any SaaS provider, and no platform provider should be able to tamper with queries to an on-chain PKI used by an end-to-end encrypted chat program. The "web3 superpowers" end up successfully crossing over the boundaries of the blockchain.

2. **Decentralized protocols compose into dApps off-chain.** Decentralized protocols with blockchain-backed security should not primarily compose using on-chain constructs such as standardized smart-contract interfaces. Instead, they are composed in off-chain programs like mobile apps and frontends to create a "full-stack" decentralized system.&#x20;

For example, clients for a decentralized encrypted chat platform with cryptocurrency payments can be built from a Sybil-resistant DHT, a blockchain-backed naming system, a micropayment network, etc, all of which are protocols trustlessly compose with on-chain logic to provide robust decentralized security. But none of these systems need to have interoperating on-chain logic.&#x20;

<figure><img src="https://lh4.googleusercontent.com/nFaHg6RtvtYZ1KY056l_SPLlzLVwpRsR8rXJ2-eCL8EdQf2oRO50ikgBEuit83N5aXWiln7UfTvjvVBxAo4Xx1aLKU2vJvXNC4FTf_9dwJjrBXtJ_brvgFP_vRhXWKUi-tty52nS1tneyXty8MCDn_3kXA=s2048" alt=""><figcaption><p>A “tech tree” of a Mel-powered world</p></figcaption></figure>

## Why do we need a new blockchain?

Why do we need a new blockchain to build out this vision?

This is because Mel is a clean-slate design revolving around one key feature --- **trustless, powerful light clients**.

This means that code running on apps outside the blockchain can _know what's going inside the blockchain_ without trusting RPC providers like Infura. We can finally take crypto superpowers out of the on-chain ecosystem and into the real world, where people, apps, and devices actually live --- without stuffing the real world onto the on-chain world!

Mel will be used as a piece of _low-level internet infrastructure_, just like the Internet Protocol or DNS. Decentralized secure protocols then pass the superpowers to user-facing apps, and a great example is Earendil (<https://earendil.network>), a decentralized, uniquely censorship-resistant communication protocol we are building as the core communication system for the Mel ecosystem.

## An overview of the docs

Throughout this website, you'll find

- A wiki on key Mel concepts from our [TXO-based data model](concepts/data-model.md) to [Melnet](concepts/melnet-the-p2p-layer.md), our HTTP-based P2P layer
- Guides on developer-oriented tasks like [building an off-chain composable protocol](developer-guides/gibbername/) and [minting MEL](developer-guides/melmint/getting-tokens/using-melminter.md)
- Resources like a [FAQ](resources/page-3.md) and [yellow paper](resources/yellow-paper.md)
