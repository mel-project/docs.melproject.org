# Decentralization beyond blockchains

## The current state of Web3

The current Web3 community seems like it's heading to a bright future. By combining incentives and cryptography, blockchains have achieved unprecedented decentralization and security, and general-purpose L1s like Ethereum now host growing ecosystems of interoperable decentralized protocols. There's everything from DeFi staples like the Compound lending platform to fascinatingly novel experiments like the cryptography game Dark Forest. Key technical problems like execution scalability now have breakthrough solutions like rollups and zero-knowledge proofs. And even the UX is steadily improving, allowing more and more regular users to join the "crypto" world.

But on a closer look, there's a big problem: **web3's revolution is stuck inside blockchains**. Smart contracts running on blockchains may have the strong neutrality, decentralization, and censorship-resistance that Web3 promises, but everywhere else, Web2's problems seem to crop up with a vengeance.

Nowhere is this clearer than in "dApp frontends": off-chain programs used to interact with on-chain systems by inherently off-chain humans and devices. The typical dApp frontend is nakedly a Web2 service, with a centralized server hosting a webpage (like [app.uniswap.org](https://app.u)) calling a centralized SaaS service to interact with on-chain smart contracts on the user's behalf. And unsurprisingly, users are at the mercy of the same sort of unaccountable centralized power we see in Web2 — just witness how a single regulatory agency (OFAC) easily cut off access to a widely-used protocol (Tornado Cash) simply by putting pressure on a few service providers (Infura, etc).

Moreover, there's a whole world of entirely non-blockchain projects that are arguably Web3 protocols — "alt-tech" protocols sharing the same goal of fixing Web2's problems with decentralized cryptography: Tor, Freenet, Matrix, and the like. Compared to blockchain apps, nearly all struggle to gain adoption due to the difficulty of successfully combining decentralization, usability, and security, a lack that blockchains seem ideally suited to fill. Yet puzzlingly, we don't see much blockchain adoption in these projects that truly need decentralized security.

## The solution: off-chain composability

Our solution to this problem is a new paradigm for the blockchain ecosystem: **off-chain composability**. This means two things:

* _Off-chain protocols compose with on-chain logic_:&#x20;
* _Decentralized protocols compose into dApps off-chain_:

##
