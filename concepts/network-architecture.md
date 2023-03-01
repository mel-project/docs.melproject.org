# Network architecture

In Mel, participants in the network can be divided into three roles:

<figure><img src="../.gitbook/assets/architecture.png" alt=""><figcaption></figcaption></figure>

* **Staker nodes** are full nodes directly participate in the proof-of-stake [consensus](consensus.md). They have stake, denominated in SYM, locked up on-chain, and receive consensus voting power in exchange.
* **Replica nodes** are full nodes that do not have SYM stake, but replicate and validate the output of the staker consensus. They not only provide a “CDN” for the blockchain, but more importantly by using a [nuking procedure](consensus.md#stick-slashing-and-nuking), they shut the network down if a quorum of stakers produces invalid results.
* **Light clients**, also known as "clients", merely _consume_ the security enforced by the stakers and replicas. They do not replicate the blockchain, but trust consensus proofs provided by the stakers that commit to a particular blockchain state. As long as the network as a whole is working correctly, light clients cannot be fooled. [Trustless light clients](light-clients.md), embedded into apps as libraries (like [melprot](../developer-guides/gibbername/melprot-a-quick-intro.md)), are the cornerstone of Mel's off-chain composable vision.

{% hint style="info" %}
**Note**: "stakers" are called "validators" in most other blockchains. We intentionally use a different word because:

* _Staker_ normalizes self-staking rather than pooled staking, and makes it clear that delegating stake to a staker is equivalent in trust to giving them a loan.
* _Validator_ is highly misleading, since the main purpose of consensus is not to "validate" blocks, but to produce a  them.&#x20;
{% endhint %}

The staker and replica nodes form a P2P gossip network using the [HTTP-based melnet protocol](network-protocol.md); light clients can also talk the same protocol to query these nodes.\
