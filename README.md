---
description: 'Off-chain composability: our vision for a Web3 far beyond blockchains'
---

# Web3 beyond blockchains

## 1/4 - Web3 is trapped on-chain

Web3 looks like it's heading into a bright future. By combining incentives and cryptography, blockchains have achieved unprecedented decentralization and security, and general-purpose L1s like Ethereum now host growing ecosystems of interoperable decentralized protocols. There's everything from DeFi staples like the Compound lending platform to fascinatingly novel experiments like the cryptography game Dark Forest. Key technical problems like execution scalability now have breakthrough solutions like rollups and zero-knowledge proofs. And even the UX is steadily improving, allowing more and more regular users to join the "crypto" world.

But on a closer look, there's a big problem: **web3's revolution is trapped inside blockchains**. Smart contracts running on blockchains may have the strong neutrality, decentralization, and censorship-resistance that Web3 promises, but everywhere else, Web2's problems seem to crop up with a vengeance.

<figure><img src="https://lh6.googleusercontent.com/5hjXD0MECAaWMoCLmJtvB2F_K0RfC4lhbdnZdTL44c7MROXfLVF45yngnX6F46UmZKfgLTSL971M4gcVMDbc0zBIVXe_z2q5xkiTeMnHpzCsPP2dwR4XrqCIzPUtLDhWUPenVhi-65S2WqErjE1ee0T1wQ=s2048" alt=""><figcaption></figcaption></figure>

### Web3 gateways

Nowhere is the lack of "Web3 superpowers" clearer than in dApp frontends: off-chain programs used to interact with on-chain systems by inherently off-chain humans and devices. The typical dApp frontend is nakedly a Web2 service, with a centralized server hosting a webpage (like [app.uniswap.org](https://app.u)) calling a centralized SaaS service to interact with on-chain smart contracts on the user's behalf.&#x20;

Unsurprisingly, users are at the mercy of the same sort of unaccountable centralized power we see in Web2 — just witness how a single regulatory agency (OFAC) easily cut off access to a widely-used protocol (Tornado Cash) simply by putting pressure on a few service providers (Infura, etc).

All of the smart contracts involved in not only Tornado Cash, but also Wormhole and Merit Circle, ran safely and correctly. But the apps—which include inherently off-chain frontends, infrastructure, etc—did not.

### Non-blockchain Web3

Moreover, there's a whole world of entirely non-blockchain projects that are arguably Web3 protocols — "alt-tech" protocols sharing the same goal of fixing Web2's problems with decentralized cryptography: Tor, Freenet, Matrix, and the like.&#x20;

Compared to blockchain apps, nearly all non-DeFi dApps struggle to gain adoption due to the difficulty of successfully combining decentralization, usability, and security, a need that blockchains seem ideally suited to satisfy. Yet puzzlingly, we don't see much blockchain adoption in these projects that truly need decentralized security.

### Mainstream Web2

And of course, mainstream internet users at the mercy of abusive Web2 desperately need Web3 censorship resistance, decentralization, and user-aligned incentives! But most of them don't know much more about Web3 beyond "crypto trading"...



## 2/4 - Mel frees web3

We believe that the way to free Web3 is a new paradigm for Web3: **off-chain composability**. Current blockchains host complex on-chain ecosystems of smart contracts with "frontends" that struggle to interface with them. Instead, we envision **full-stack** off-chain dApps leveraging Mel as the **web3 superpower** **component**, not as a platform.

\


<figure><img src="https://lh5.googleusercontent.com/qYoe5Gvu51c5Jhu_Cr6Z0DmBbQs5bFHov_mx6EuAAVLtaaczghWcmpFIgjXUq3MiJ_bRA23gRIn_7Kd9ynvb10h8aPiinRqbBWQGV_A4R8L-IRbp_HrVs81sD_vEfas5ooQjLs0iuk-tO29Njh2NeASesA=s2048" alt=""><figcaption></figcaption></figure>







This will be a largely non-blockchain world with two key properties:

**Off-chain programs trustlessly compose with on-chain programs.** Off-chain programs, whether dApp frontends or whole alt-tech stacks, must integrate on-chain functionality without sacrificing L1 security.&#x20;

For instance, a frontend for a coin mixer can't be censorable by any SaaS provider, and no platform provider should be able to tamper with queries to an on-chain PKI used by an end-to-end encrypted chat program. The "web3 superpowers" end up successfully crossing over the boundaries of the blockchain.

**Decentralized protocols compose into dApps off-chain.** Decentralized protocols with blockchain-backed security should not primarily compose using on-chain constructs such as standardized smart-contract interfaces. Instead, they are composed in off-chain programs like mobile apps and frontends to create a "full-stack" decentralized system.&#x20;

For example, clients for a decentralized encrypted chat platform with cryptocurrency payments can be built from a Sybil-resistant DHT, a blockchain-backed naming system, a micropayment network, etc, all of which are protocols trustlessly compose with on-chain logic to provide robust decentralized security. But none of these systems need to have interoperating on-chain logic.&#x20;

<figure><img src="https://lh4.googleusercontent.com/nFaHg6RtvtYZ1KY056l_SPLlzLVwpRsR8rXJ2-eCL8EdQf2oRO50ikgBEuit83N5aXWiln7UfTvjvVBxAo4Xx1aLKU2vJvXNC4FTf_9dwJjrBXtJ_brvgFP_vRhXWKUi-tty52nS1tneyXty8MCDn_3kXA=s2048" alt=""><figcaption><p>A “tech tree” of a Mel-powered world</p></figcaption></figure>

_TODO: Explain how this solves all the issues_

## 3/4 - Why do we need a new blockchain?

But why do we need a new blockchain to implement off-chain Web3? Can't we build this world on an established blockchain like Ethereum?

It turns out that ease of off-chain composability requires many design trade-offs in all the aspects of a blockchain, and features for on-chain ecosystems hamper both **exporting** and **producing** Web3 superpowers. As examples:

#### VM improvements

These provide easy opcodes for cool stuff like ZK and make smart contracts happy by saving gas costs. However, the governance involved both breaks thin client compatibility and threatens neutrality

#### Standard contract APIs

Decoupling contract interface from implementation, these APIs power many on-chain ecosystems (ERC-20 DeFi etc). However, the disparate implementations complicate off-chain verification, and buggy/centralized contracts introduce systemic security risks.

#### "Moar" TPS

This makes on-chain things fast and cheap, essentially making a bigger box for bigger contracts. However, thin clients are way harder to run, and bigger nodes lead to centralization.



Unfortunately, existing blockchains double down on these features, trading away off-chain composability for an on-chain ecosystem. That's why we made a new L1 from scratch, optimizing every part of Mel for being the **decentralized security keystone** that enables an off-chain composable Web3.



## 4/4 - Up next: how Mel frees web3

We'll dive into how key components of Mel are designed for off-chain composability (specifically, **producing** and **exporting** Web3 superpowers) in subsequent pages:

* The synergy between composability and neutrality in Mel's data model: We use a stripped-down TXO-based model optimized for trustless off-chain queries that is intended to be simple enough to be _governance-free_ and thus robustly neutral. This turns out to both support and require a vibrant off-chain composable ecosystem.
* The consensus game: Mel's proof-of-stake consensus has two important features, both crucial to reliable off-chain thin-clients. First, it uses unique _collusion-resistant incentives_, ensuring long-run safety without external governance intervening. Second, it produces _long-range consensus proofs_ that minimizes "weak subjectivity" problems and allows for deeply embedded thin clients to stay economically secure.
* The mechanism, Melmint, behind the native currency MEL: Without any oracles or external trust, Melmint stabilizes MEL's value against the _cost of sequential computation time_, one of the few purchasing-power-stable indices that can be trustlessly defined and measured. This makes MEL as trustless as BTC and ETH, while still being a useful unit of account that avoids typical cryptocurrency price swings. It turns out that this is key to building off-chain composable financial systems.

