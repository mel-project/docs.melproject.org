# Gibbername: your first off-chain composable protocol

## On-chain vs. off-chain composability

The best way of understanding **off-chain composability**, Mel's new paradigm for the blockchain ecosystem, is to contrast it with the way Web3 is right now. Right now, smart contracts using standard APIs like ERC-20 form a pretty nice interoperable on-chain ecosystem, enabling applications like complex DeFi instruments beyond what's possible with first-generation blockchains like Bitcoin.

But interacting with anything _outside_ this smart contract ecosystem is really difficult.For instance, it's very easy to integrate ENS into an on-chain contract, but much harder to use it as a decentralized replacement to DNS in a way without sacrificing security (e.g. by using DNS gateways) or usability (e.g. forcing everybody to run their own full node to look up names).

And regardless of how much blockchains scale and how far we make protocols "crypto-native", we can't get around crossing the on-chain/off-chain boundary. Any web3 ecosystem has components like frontends, apps, and _human users_ that fundamentally live off-chain while needing to securely talk to on-chain.

```
[picture illustrating this. expanding bubble surrounding more and more of web3, but the boundary of the bubble still sucks]
```

On the other hand, we believe that a truly successful Web3 requires a rich ecosystem of secure decentralized protocols and apps outside of the blockchain. This of course requires a blockchain --- Mel --- that actually supports use as an off-chain root of trust through features like a simple-to-implement protocol, a governance-free development model, and most importantly embeddable thin clients that let off-chain programs efficiently query blockchain info without sacrificing security.

More interestingly, on-chain logic for decentralized protocols plays a very different role. Instead of interoperating with on-chain code, _on-chain protocols are written for off-chain consumers_. Mel's overall data model lacking the entire concept of smart contracts calling each other, but it is supremely suited for encoding on-chain logic in a way that is legible and usable off-chain.

## What will we build?

To demonstrate that, we'll build **Gibbername**, a trivial DNS-like decentralized naming system that would serve as an exemplary citizen of an off-chain composable ecosystem. Gibbername allows you to

- **Register** a short, human-readable name. You won't be able to pick the name though, it will look something like `sublak-demfet`. (As we'll see, this makes the implementation really simple)
- **Bind** the name to any data you wish, like a DNS record or JSON document.
- **Transfer** the name to another person.

Like DNS, Gibbername will be very easy to integrate into apps, with a simple Rust library that allows for looking up and managing names. Unlike DNS, though, Gibbername will have three important, blockchain-backed "superpowers":

- **Identity retention**: without the consent of the current owner, nobody can change the name-to-data mapping or transfer the name
- **Censorship resistance**: nobody is able to prevent from name from resolving or prevent the current owner from updating or transferring the name
- **Permissionlessness**: nobody can stop name registrations

All the above properties will be upheld despite the fact that lookups, transfers, etc will all be initiated by off-chain code rather than on-chain contracts.
