---
description: This is your one-stop shop for all commonly asked questions.
---

# Frequently asked questions

{% hint style="info" %}
This FAQ is intended to be perpetually WIP. Feel free to add your own Q\&A's in a pull request!
{% endhint %}

### What is Mel?

Mel is a minimal, governance-free L1 blockchain whose core vision consists of enabling a future ecosystem of secure, composable, off-chain dapps. Learn more in our [intro](../).

### What are the considerations for decentralization, security, and scalability?

In accordance with Mel's core values of decentralization and trustlessness, the core security of the protocol is in the hands of stakers, for which there is no minimum staking amount and low hardware requirements, reducing the barrier to entry. Additionally, though stakers in other chains are often assumed to be honest (and therefore not prone to collusion), Mel eschews these assumptions and instead uses the rigorous consensus algorithm Synkletos, which operates under the assumption that stakers _will_ collude, and bakes this into its own security.

Mel's philosophy of focusing on off-chain, composable dapps is central to its scaling strategy. This is made possible through the MelVM, a non-Turing-complete virtual machine that nevertheless is able to compute all primitive recursive functions and enables attaching sophisticated covenants to coins. This empowers scaling strategies crucial to global adoption, such as full nodes with limited storage space and thin clients that can securely verify much more information than conventional techniques like Bitcoin’s SPV allows, while at the same time avoiding the pitfalls of stateful smart contracts.

### How does MEL stabilize itself?

MEL is cryptographically pegged to a DOSC (day of sequential computation) using the Melmint algorithm. The DOSC was chosen because it has a relatively stable purchasing power and because it is trustlessly measurable through a sequential proof of work. This, combined with a protocol-internal Uniswap-style automated market maker (AMM), provides incentives which allow MEL to self-stabilize. Get a more in depth view of the Melmint algorithm [here](../developer-guides/melmint/getting-tokens/minting-mel-with-melminter.md).

### How is MEL different from other stablecoins like USDC or DAI?

Apart from not requiring any trust in centralized entities (like USDC), MEL differentiates itself from the crowd by not only being decentralized (like DAI), but by _also_ not being pegged to any fiat currencies, which themselves are subject to centralized influence and manipulation.

### What are the risks of putting my money into Mel? What are the failure scenarios for the network?

There are a couple of failure scenarios which we can imagine with regard to the Melmint algorithm:

* A drastic change in DOSC value would cause MEL to lose its stable purchasing power, although historically this has not happened and is unlikely to in the future due to the definition of a DOSC in terms of _time_ rather than _amount_ of computation.
* A sudden decrease in Mel market sentiment, in say, a general cryptocurrency crash, could result in an extremely rare scenario where a large amount of SYM or MEL is simultaneously panic-sold; this would threaten the basis of the Melmint peg but is unlikely to completely dump MEL due to Melmint's design. This is especially because, unlike users of USD stablecoins, MEL users never expected zero exchange-rate risk and are unlikely to completely dump MEL due to a temporary depeg. The economic impact of such a depeg is likely to be around the same order of magnitude as a fiat “currency crisis” — bad, but not catastrophic. Once sufficient liquidity returns to the Melswap MEL/SYM market, the peg will gradually be restored.

### How much does it cost to run a staker node? Is it profitable?

There is no minimum amount of SYM which needs to be staked in order to participate in consensus. Profit from staking comes from transaction fees present in each block. More information on staking can be found [here](broken-reference).

### How can I get some MEL?

Currently, you can only acquire MEL by running a `melminter` instance or by swapping SYM or ERG via Melswap. Get more information about acquiring tokens [here](../developer-guides/melmint/getting-tokens/).
