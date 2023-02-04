# Gibbername: your first off-chain composable protocol

## On-chain vs. off-chain composability

The best way of understanding **off-chain composability**, Mel's new paradigm for the blockchain ecosystem, is to contrast it with the way Web3 is right now. Right now, smart contracts using standard APIs like ERC-20 form a pretty nice interoperable on-chain ecosystem, enabling applications like complex DeFi instruments beyond what's possible with first-generation blockchains like Bitcoin.

But interacting with anything outside this smart contract ecosystem turns out to be _really_ hard. For instance, let's say you want to heard that ENS is decentralized and trustless, so you want to use it to replace DNS in some off-chain decentralized protocol (maybe to name Tor hidden services?).&#x20;

You might think that it'll be a nice drop-in replacement, since it's very easy to integrate ENS into on-chain contracts. But you would be very wrong: the names are stored "illegibly" in the ENS contract state, exposed only through an on-chain contract API. All of your options for calling this API from off-chain code are bad:

* **Sacrifice security:** just call RPC methods on some full node (with something like Web3.js), who you end up trusting completely. Just like with DNS, a centralized third party can now arbitrarily lie to you, censor you, etc. Oops!
* **Sacrifice scalability**: force all your clients to run their own Ethereum full node, that you then query using the same RPC methods. This is going to be impractical for almost any application.
* **Reverse-engineer the contract and hack together a hyper-fragile thin client**: Ethereum does have _some_ thin-client support. So you can sorta-trustlessly query ENS names by disassembling the EVM code of ENS, figuring out where exactly in the blockchain state it stores name information, then craft the exact `getProof` calls needed to get the data in a verifiable way. But this is horribly brittle. The smallest ENS governance upgrade can require you to re-reverse-engineer the EVM code, not to mention L1 issues like consensus-breaking upgrades and block reorganizations. You'll never compete against DNS's reliability and simplicity, and everybody trying to integrate your protocol will think "web3 has such bad UX".

Furthermore, this "on/off-chain boundary problem" persists regardless of how much blockchains scale and how far we make protocols "crypto-native". Any web3 ecosystem has components like frontends, apps, and _human users_ that fundamentally live off-chain while needing to securely talk to on-chain.

```
[picture illustrating this. expanding bubble surrounding more and more of web3, but the boundary of the bubble still sucks]
```

We need to solve this problem to have a truly successful Web3, with a rich ecosystem of secure decentralized protocols and apps outside of the blockchain. This requires a new kind of blockchain --- Mel --- that actually supports use as an off-chain root of trust. Features like a simple-to-implement protocol, a governance-free development model, and embeddable thin clients cooperate let off-chain programs efficiently utilize on-chain security.

More interestingly, on-chain logic for decentralized protocols plays a very different role. Instead of interoperating with on-chain code, _on-chain protocols are written for off-chain consumers_. Mel's overall data model lacks the entire concept of smart contracts calling each other, but it is supremely suited for encoding on-chain logic in a way that is legible and usable off-chain.

## What will we build?

To demonstrate that, we'll build **Gibbername**, a trivial DNS-like decentralized naming system that would serve as an exemplary citizen of an off-chain composable ecosystem. Gibbername allows you to

* **Register** a short, human-readable name. You won't be able to pick the name though, it will look something like `sublak-demfet`. (As we'll see, this makes the implementation really simple)
* **Bind** the name to any data you wish, like a DNS record or JSON document.
* **Transfer** the name to another person.

Like DNS, Gibbername will be very easy to integrate into apps, with a simple Rust library that allows for looking up and managing names. Unlike DNS, though, Gibbername will have three important, blockchain-backed "superpowers":

* **Identity retention**: without the consent of the current owner, nobody can change the name-to-data mapping or transfer the name
* **Censorship resistance**: nobody is able to prevent from name from resolving or prevent the current owner from updating or transferring the name
* **Permissionlessness**: nobody can stop name registrations

All the above properties will be upheld despite the fact that lookups, transfers, etc will all be initiated by off-chain code rather than on-chain contracts.
