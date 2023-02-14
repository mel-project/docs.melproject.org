# Decentralization beyond blockchains

## The current state of Web3

The current Web3 community seems like it's heading to a bright future. By combining incentives and cryptography, blockchains have achieved unprecedented decentralization and security, and general-purpose L1s like Ethereum now host growing ecosystems of interoperable decentralized protocols. There's everything from DeFi staples like the Compound lending platform to fascinatingly novel experiments like the cryptography game Dark Forest. Key technical problems like execution scalability now have breakthrough solutions like rollups and zero-knowledge proofs. And even the UX is steadily improving, allowing more and more regular users to join the "crypto" world.

But on a closer look, there's a big problem: **web3's revolution is stuck inside blockchains**. Smart contracts running on blockchains may have the strong neutrality, decentralization, and censorship-resistance that Web3 promises, but everywhere else, Web2's problems seem to crop up with a vengeance.

Nowhere is this clearer than in "dApp frontends": off-chain programs used to interact with on-chain systems by inherently off-chain humans and devices. The typical dApp frontend is nakedly a Web2 service, with a centralized server hosting a webpage (like [app.uniswap.org](https://app.u)) calling a centralized SaaS service to interact with on-chain smart contracts on the user's behalf. And unsurprisingly, users are at the mercy of the same sort of unaccountable centralized power we see in Web2 — just witness how a single regulatory agency (OFAC) easily cut off access to a widely-used protocol (Tornado Cash) simply by putting pressure on a few service providers (Infura, etc).

Moreover, there's a whole world of entirely non-blockchain projects that are arguably Web3 protocols — "alt-tech" protocols sharing the same goal of fixing Web2's problems with decentralized cryptography: Tor, Freenet, Matrix, and the like. Compared to blockchain apps, nearly all struggle to gain adoption due to the difficulty of successfully combining decentralization, usability, and security, a need that blockchains seem ideally suited to satisfy. Yet puzzlingly, we don't see much blockchain adoption in these projects that truly need decentralized security.

## Our vision: off-chain composability

We believe that the solution to this problem is a new paradigm for "Web3": **off-chain composability**. Instead of a complex on-chain ecosystem of smart contracts with "frontends" that struggle to interface with them, this will be a largely non-blockchain world with two key properties:

_Off-chain programs trustlessly compose with on-chain programs._ Off-chain programs, whether dApp frontends or whole alt-tech stacks, must integrate on-chain functionality without sacrificing L1 security.&#x20;

For instance, a frontend for a coin mixer can't be censorable by any SaaS provider, and no platform provider should be able to tamper with queries to an on-chain PKI used by an end-to-end encrypted chat program. The "web3 superpowers" end up successfully crossing over the boundaries of the blockchain.

_Decentralized protocols compose into dApps off-chain_. Decentralized protocols with blockchain-backed security should not primarily compose using on-chain constructs such as standardized smart-contract interfaces. Instead, they are composed in off-chain programs like mobile apps and frontends to create a "full-stack" decentralized system.&#x20;

For example, clients for a decentralized encrypted chat platform with cryptocurrency payments can be built from a Sybil-resistant DHT, a blockchain-backed naming system, a micropayment network, etc, all of which are protocols trustlessly compose with on-chain logic to provide robust decentralized security. But none of these systems need to have interoperating on-chain logic.&#x20;

##
