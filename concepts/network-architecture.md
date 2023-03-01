# Network architecture

In Mel, participants in the network can be divided into:

- **Staker nodes** are full nodes directly participate in [consensus](consensus.md). They have stake, denominated in SYM, locked up on-chain, and receive consensus voting power in exchange.
- **Replica nodes** are full nodes that do not have SYM stake, but replicate and validate the output of the staker consensus. They not only provide a “CDN” for the blockchain, but more importantly by using a nuking procedure, they shut the network down if a quorum of stakers produces invalid results.
