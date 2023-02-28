# Consensus

**Consensus** refers to the procedure Mel uses to produce and decide on a canonical blockchain history.

In broad strokes:

- Mel uses a _fixed-term proof of stake_ to decide consensus participants, known as **stakers**. Stakers must stake SYM, a special PoS token, for integer multiples of 200,000-block (~70-day) **epochs**, and changes in the effective list of stakers only happen on epoch boundaries.
- Each block is decided by a Byzantine fault-tolerant (BFT) consensus algorithm that eventually produces a **consensus proof** --- signatures from stakers owning at least 2/3 of the staked SYM. This produces _immediate finality_, meaning the block can never be reverted, and Mel can never have "block reorgs". Interestingly, the exact consensus algorithm used is not part of the core protocol rules and anything can be used as long as it produces consensus proofs of the right format; the current implementation, however, uses Streamlette, an extremely simple consensus derived from Streamlet.
- The **consensus game** --- the cryptoeconomic mechanism incentivizing consensus correctness --- generally trades off other properties (like capital efficiency) for robust long-term economic security. The "carrot" of a _collusion-resistant fee economy_ allows stakers to extract value primarily through "benign collusion" on fees rather than block rewards or misbehavior. Strict **slashing**, including a catastrophic form known as "nuking", act as unusually powerful "sticks" against staker misbehavior.

## Staking and epochs

To select who gets to participate in consensus, Mel uses a proof-of-stake system. In short, anyone can lock up a sum of SYM, and while it is locked up, a designated node (the _staker_, which may or may not be the same person who locked up the SYM) receives voting power in consensus.

How does this work in practice? blockchain history is divided into 200,000-block epochs, conventionally numbered from 0. For example, block 12,345 is in epoch 0, while block 1,968,968 is in epoch 9. Anyone can stake (lock up) a particular SYM for a fixed number epochs, during which this SYM will give a designated staker voting power. This is done by sending a special transaction (of type `TxKind::Stake`, see TodoYellowPaper) with a SYM-denominated output, with the following metadata:

- the _beneficiary staker public key_ that uniquely identifies the staker who receives voting power
- the _starting epoch_ of this stake, or the first epoch in which this stake contribvutes to the staker's voting power
- the _post-end epoch of this stake_, or the epoch _after_ the last epoch that the stake contributes to the staker's voting power.

The SYM --- known as an individual **stake** --- is then locked up until the end of the the post-end epoch; after the start of the starting epoch and before the start of the post-end epoch, the staker has voting power.

To illustrate this, let's look at example. Here, Stacy is a staker node, and Alice is someone who staked 100 SYM at block height 900,000 for epochs 5..10, designating Stacy as the beneficiary:

<figure><img src="../.gitbook/assets/stake-diagram.png" alt=""></figure>
