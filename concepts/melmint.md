# MEL: trustless sound money

Despite using [SYM for proof-of-stake consensus](consensus.md), Mel's native _circulating_ currency is **MEL**. MEL is used extensively in the base-layer incentive design:

* all transaction fees must be paid in MEL
* block rewards are denominated in MEL

The most interesting feature of MEL is that it is probably the _first ever cryptocurrency to have_ **endogenous stability**. This means that:

* The way MEL is issued is entirely trustless and decentralized. Just like Bitcoin, no oracles, governance DAOs, or issuers are involved.
* Nevertheless, MEL maintains a stable purchasing power.

## Why endogenous stability?

Why do we want something like this in the first place?

First of all, volatile cryptocurrency prices actually do pose a serious problem. The value of a currency going up and down erratically makes it bad as money: it becomes less useful as a store of value or unit of account, and it greatly damages the ability to create interesting financial instruments. Nobody is going to sign a 30-year mortgage in Bitcoin without knowing whether Bitcoin will be worth 1, 1,000, or 1,000,000 apples 30 years in the future.&#x20;

This also greatly impacts mechanism design and DeFi --- the ecosystem relies heavily on fiat-pegged stablecoins like USDC and Dai. But these stablecoins are not what we want: they inherently rely on exogenous trust, in oracles, central issuers, etc. Note that this is a problem even when the stablecoin aims to track a basket of commodities rather than fiat currency — we may lose centralized trust in the Fed, but not trust in oracles. Oracles supplying external information about the market are crucial for the feedback loop of any stablecoin backed by off-chain assets, and they rely heavily on preexisting trust in people or institutions, not endogenous trust guaranteed by protocol incentives.

Thus, we uses an oracle-free system that doesn’t try to peg MEL to any external asset. Instead, we define a new, _trustlessly-measurable value unit_, called the **DOSC (day of sequential computation)**, which can be measured on-chain by an autonomous mechanism with full endogenous trust. This, and not dollars or gold, is what the core mechanism, called Melmint, then pegs MEL to.

## How does Melmint work?

The one-sentence summary of Melmint’s job is to maintain **peg 1 MEL to 1 DOSC**.&#x20;

### The peg target: 1 DOSC

A “DOSC” is a “day of sequential computation”. It’s defined as the _cost of running a sequential computation for 24 hours, using the fastest processor available_. For example, a DOSC in the year 2000 is the cost of occupying the fastest single CPU core _available in 2000_ for 24 hours, while a DOSC in the year 2021 is the cost of doing the same with a 2021 processor.

The DOSC has two really cool properties that make it a great target for a peg:

* It has a relatively stable purchasing power. Empirically, the “fastest processor” typically costs about the same, despite its performance drastically increasing over time. We explore this further in our [DOSC analysis data](https://github.com/themeliolabs/dosc-analysis).
* More importantly, it’s _trustlessly measurable_ through a “sequential proof of work”. A hash-based sequential proof of work, like the one invented by Cohen and Pietrzak, produces a succinct proof that starting from some seed `x`, a large amount of sequentially nested hashes `H(H(H(H(...H(x)))))` have been computed. By using a cleverly incentivized on-chain benchmark, we can then measure how fast the fastest processor is. This gets us a way of showing on-chain that we’ve wasted a day’s worth of sequential computation, letting us measure the DOSC with strong endogenous trust. The DOSC’s trustless measurability is the _key to Melmint’s oracle-free mechanism_.

### How is this peg maintained?

There are two main steps to actually pegging the MEL to DOSC: _erg-minting_ and the _central mechanism_, summarized in the following picture:

\


\
