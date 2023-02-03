# Your first off-chain composable protocol

_Briefly describe the paradigm_

- _Old "on chain composability" model:_&#x20;
  - _protocol's consumers are on-chain entities like accounts, other contracts, etc_
  - _integrating with other on-chain protocols:_ :relaxed:\_\_
  - _integrating with anything off-chain or cross-chain:_ :angry:**:exploding_head:**:angry:\_\_
- _New "off chain composability" model:_
  - _protocol's consumers are off-chain protocols, apps, etc._
    - _filesharing apps_
    - _websites_
    - _games_
    - _etc_
  - _easily gives blockchain-backed secure decentralization to off-chain systems_

## On-chain vs. off-chain composability

The best way of understanding **off-chain composability**, Mel's new paradigm for the blockchain ecosystem, is to contrast it with _on-chain composability_, the dominant Web3 development model.

Right now, a "web3 protocol" usually refers to an on-chain smart contract with an on-chain API (in some language like Solidity) that can be called from other contracts. For instance, Compound, ENS, etc. The resulting ecosystem of interoperating, composable smart contracts has turned out to be very successful. New techniques such as rollups and sharding even promise to scale the performance of this ecosystem far beyond that of a conventional blockchain.

But interacting with anything _outside_ this smart contract ecosystem is really difficult. This "web3 boundary problem" goes far deeper than complaints about clunky Web3 UX. It is a pervasive failure of on-chain code (smart contracts) and off-chain code (RPC networks, wallets, frontends, etc) to interact in a way that seamlessly preserves the decentralized, incentive-backed security that is the whole reason why web3 exists. For instance, it's very easy to integrate ENS into an on-chain contract, but much harder to use it as a decentralized replacement to DNS in a way without sacrificing security (e.g. by using DNS gateways) or usability (e.g. forcing everybody to run their own full node to look up names).

And scaling blockchains so that more things can be "web3 native" and run on-chain doesn't really fix this. Almost any web3 app has components like frontends, apps, and _human users_ that are are fundamentally off-chain and cannot be running inside any smart contract environment, no matter how fancy that environment is.

```
[picture illustrating this. expanding bubble surrounding more and more of web3, but the boundary of the bubble still sucks]
```

Off-chain composability, on the other hand, focuses on building a rich Web3 ecosystem outside of the blockchain. The Mel blockchain's role is then to support the decentralized security of this non-blockchain world, by hosting on-chain logic that acts as a trustless and immutable source of truth.

This means that on Mel, _on-chain protocols are designed for off-chain consumers_. Mel's overall data model lacking the entire concept of smart contracts calling each other on one hand. But by using tricks like extensively Merkle-indexing its programmable UTXO graph, it is supremely suited for encoding on-chain logic in a way that is legible and usable off-chain through trustless thin-clients.

## What will we build?

In this tutorial, we'll build **Gibbername**, a trivial DNS-like decentralized naming system that would serve as an exemplary citizen of an off-chain composable ecosystem. Gibbername allows you to

- **Register** a short, human-readable name. You won't be able to pick the name though, it will look something like `sublak-demfet`. (As we'll see, this makes the implementation really simple)
- **Bind** the name to any data you wish, like a DNS record or JSON document.
- **Transfer** the name to another persion.

Like DNS, Gibbername will be very easy to integrate into apps, with a simple Rust library that allows for looking up and managing names. Unlike DNS, though, Gibbername will have three important, blockchain-backed "superpowers":

- **Identity retention**: without the consent of the current owner, nobody can change the name-to-data mapping or transfer the name
- **Censorship resistance**: nobody is able to prevent from name from resolving or prevent the current owner from updating or transferring the name
- **Permissionlessness**: nobody can stop name registrations

All the above properties need to be upheld despite lookups, transfers, etc are initiated by off-chain code through Mel thin clients rather than on-chain contracts.

> Hint: will be hard on other blockchains etc
