---
description: >-
  A high-level overview of the different developer tools available to get
  started participating in the Themelio ecosystem.
---

# Overview

Themelio is currently in _beta mainnet_: there is a "[mainnet](https://scan.themelio.org/)" with relatively stable and persistent history, but the staking tokens are not publicly available and the protocol is not quite production-ready.

Nevertheless, there is already a rich developer toolkit for interacting with the blockchain.

## Themelio's architecture

But before diving into the specific tools, it's helpful to keep in mind the overall architecture of Themelio.

\[ a picture illustrating the whole architecture]

Participants in the Themelio blockchain network itself can be roughly divided into (full) **nodes** and (thin) **clients**. Nodes replicate every block and transaction on the blockchain and help maintain network security. A subset of nodes, **staker nodes**, have SYM locked up and participate in the consensus to decide canonical blockchain history. All other nodes are **replica nodes** that replicate and verify blocks but do not propose new blocks.

Clients do not replicate any blocks, but are able to interact with the blockchain state by asking full nodes. An important design principle is that _clients do not trust full nodes_: full nodes must present proof that the info sent to client is part of canonical blockchain history.

**Apps** are generally built on top of Themelio thin clients. The most basic app is probably a **wallet**, a tool for managing on-chain assets and "manual" interaction with blockchain state. More complex applications can be built by composing on-chain logic with off-chain functionality using trust-minimizing thin clients; this is the cornerstone of Themelio's off-chain composability vision.

## Tooling overview

Currently, the following tools are available:

### Full nodes

Both staker and replica nodes are supported by **melnode**, our official node software. A basic guide is currently available.

Staking requires SYM, which is currently available only on the testnet. You can follow our testnet staking guide to learn how to run a staker node.

### Thin clients

We have a full-featured thin client library available in **melprotocol**, our protocol crate. You can find detailed documentation on docs.rs.

Also available is an introductory guide to building your first trustless off-chain app using a Themelio thin client.

### Wallets

We have an official, feature-complete CLI reference wallet called **melwallet-cli**.

There is also an alpha-quality GUI wallet [Mellis](https://github.com/themeliolabs/mellis), but features may be missing or broken.

### On-chain development

You can deploy on-chain logic using our high-level covenant programming language [Melodeon](https://melodeonlang.org/).

### Melminter

TODO
