# MEL: trustless sound money

Despite using [SYM for proof-of-stake consensus](consensus.md), Mel's native _circulating_ currency is **MEL**. MEL is used extensively in the base-layer incentive design:

* all transaction fees must be paid in MEL
* block rewards are denominated in MEL

The most interesting feature of MEL is that it is probably the _first ever cryptocurrency to have_ **endogenous stability**. This means that:

* The way MEL is issued is entirely trustless and decentralized. Just like Bitcoin, no oracles, governance DAOs, or issuers are involved.
* Nevertheless, MEL maintains a stable purchasing power.

## Why endogenous stability?

Why do we want something like this in the first place?

First of all, volatile cryptocurrency prices actually do pose a serious problem. The value of a currency going up and down erratically makes it bad as money: it becomes less useful as a store of value or unit of account, and it greatly damages the ability to create interesting financial instruments. Nobody is going to sign a 30-year mortgage in Bitcoin without knowing whether Bitcoin will be worth 1, 1,000, or 1,000,000 apples 30 years in the future.

This also greatly impacts mechanism design and DeFi --- the ecosystem relies heavily on fiat-pegged stablecoins like USDC and Dai. But these stablecoins are not what we want: they inherently rely on exogenous trust, in oracles, central issuers, etc. Note that this is a problem even when the stablecoin aims to track a basket of commodities rather than fiat currency — we may lose centralized trust in the Fed, but not trust in oracles. Oracles supplying external information about the market are crucial for the feedback loop of any stablecoin backed by off-chain assets, and they rely heavily on preexisting trust in people or institutions, not endogenous trust guaranteed by protocol incentives.

Thus, we uses an oracle-free system that doesn’t try to peg MEL to any external asset. Instead, we define a new, _trustlessly-measurable value unit_, called the **DOSC (day of sequential computation)**, which can be measured on-chain by an autonomous mechanism with full endogenous trust. This, and not dollars or gold, is what the core mechanism, called Melmint, then pegs MEL to.

## How does Melmint work?

The one-sentence summary of Melmint’s job is to maintain **peg 1 MEL to 1 DOSC**.

### The peg target: 1 DOSC

A “DOSC” is a “day of sequential computation”. It’s defined as the _cost of running a sequential computation for 24 hours, using the fastest processor available_. For example, a DOSC in the year 2000 is the cost of occupying the fastest single CPU core _available in 2000_ for 24 hours, while a DOSC in the year 2021 is the cost of doing the same with a 2021 processor.

The DOSC has two really cool properties that make it a great target for a peg:

* It has a relatively stable purchasing power. Empirically, the “fastest processor” typically costs about the same, despite its performance drastically increasing over time. We explore this further in our [DOSC analysis data](https://github.com/themeliolabs/dosc-analysis).
* More importantly, it’s _trustlessly measurable_ through a “sequential proof of work”. A hash-based sequential proof of work, like the one invented by Cohen and Pietrzak, produces a succinct proof that starting from some seed `x`, a large amount of sequentially nested hashes `H(H(H(H(...H(x)))))` have been computed. By using a cleverly incentivized on-chain benchmark, we can then measure how fast the fastest processor is. This gets us a way of showing on-chain that we’ve wasted a day’s worth of sequential computation, letting us measure the DOSC with strong endogenous trust. The DOSC’s trustless measurability is the _key to Melmint’s oracle-free mechanism_.

### How is this peg maintained?

There are two main steps to actually pegging the MEL to DOSC: _erg-minting_ and the _central mechanism_, summarized in the following picture:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Erg-minting involves allowing anybody to use a special transaction type (`DoscMint`) to prove that they completed a certain amount of sequential proof-of-work. This transaction then generates $$k$$ "erg" for each DOSC of work done. $$k$$ here is not a constant, but an exponentially increasing conversion factor --- while 1 DOSC of work may generate $$10$$ erg today, it might generate $$100$$ erg a year from now. Due to this rapid inflation, erg act as an on-chain, tokenized representation of _recent_ sequential work.

The central mechanism uses **Melswap**, a built-in, Uniswap-like decentralized exchange that supports all Mel-based tokens. Its objective is to _peg 1 MEL to 1 DOSC worth of SYM_, using a feedback loop that prints mels and buys syms or vice-versa. Recall that SYM is Mel's separate, proof-of-stake token. First, we read two exchange rates off of Melswap:

* $$s$$: how much SYM can 1 MEL buy
* $$t$$: how much SYM can 1 erg buy

Then, Melswap targets an exchange rate of 1 MEL = $$tk$$ SYM --- that is, 1 DOSC ($$k$$ erg) worth of syms. When this $$s=tk$$ peg fails to hold, we use _inflation_ to back it:

* **Mel too cheap**: This is the blue box in the picture, where $$s<tk$$. In this case, Melmint continually prints syms out of thin air, using them to buy MEL, which are subsequently destroyed. This artificially increases the demand for MEL, increasing its purchasing power until $$s=tk$$.
* **Mel too expensive**: This is the red box in the picture, where $$s>tk$$. Here, Melmint would instead print mels, using them to buy syms. This increases the supply of MEL, decreasing its price until $$s=tk$$.

We now take a look at each individual step in this process:

### Step 1: Minting erg

The first step in Melmint is for minters --- which can be anybody --- to mint **erg**, $$k$$ of which represent a DOSC.

#### Proving work

Minters create erg through a DoscMint transaction, something detailed in the \[yellow paper]\(\{{< relref path="../specifications/yellow.md#applying-special-actions" >\}} "Yellow Paper").

In summary, the `data` field of a DoscMint transaction contains two values: a _difficulty exponent_ $$z$$, as well as a _proof_ $$\pi$$. This uses MelPoW, a non-interactive proof-of-sequential-work system whose details aren't important for this document, but in short, $$(D,\pi)$$ is a proof that roughly $$2^z$$ nested hashes have been computed on the _seed_ $$\chi$$, which consists of the first input to the transaction, hashed together with the block hash of the block in which that first input was confirmed.

The key property here is that _the minter cannot predict_ $$\chi$$ _before the first input to the transaction is confirmed_. This means that the minter must have finished the $$2^z$$ hashes after the first input has confirmed, but before the DoscMint transaction itself has confirmed. We now have a time interval, and therefore a _trustless measure of the minter's speed in hashes per second_.

By simply remembering the fastest minter ever seen, the blockchain knows how many hashes the fastest minter can do in a day; let's call that $$M$$. Then, we know that our minter did $$d=2^z/M$$ DOSC of work --- that is, the same amount of sequential work as the fastest minter run for $$2^z/M$$ days would do.

#### Creating ergs

Now, the DoscMint transaction has proven that the minter did $$d$$ DOSCs of work. The blockchain needs to award the minter with $$k d$$ erg (where $$k$$ is the erg-DOSC conversion factor) --- in Mel's coin-based model, this is simply done by allowing imbalance in incoming and outgoing erg: the transaction's allowed to produce $$k d$$ more erg than it consumed.

**Important note**: $$k$$ here is an exponentially increasing conversion factor: $$k=1$$ at the genesis block, and then $$k$$ increases by 0.00005% every block. This translates to approximately a 70% annual inflation rate. The purpose of an exponentially increasing $$k$$ is so that _the supply of erg is dominated by freshly minted ergs_ --- this ensures that the market value of $$k$$ erg tracks the 1 DOSC cost of creating those ergs. If $$k$$ were instead constant, a drop in demand would cause a glut of old erg that would not sell, where $$k$$ erg become worth less than 1 DOSC despite them costing 1 DOSC when they were minted. This would make erg useless as way of quantifying the value of a DOSC.

#### Selling the ergs

Because of the astronomical inflation rate, ergs are not very useful as money. Instead, minters would almost always want to sell their ergs soon for a more value-stable asset.

Mel conveniently comes with a built-in decentralized exchange, called Melswap, where any Mel-based token (including custom tokens) can be exchanged for any other. This, of course, includes ergs. Minters can easily take their ergs and exchange them for other tokens, most likely the two main Mel tokens Sym and Mel.

In particular, the SYM/erg market is "special" --- it is crucial for driving Melmint's core pegging mechanism, and in fact it's subsidized by diverting half of all SYM block rewards to the SYM/erg Melswap pool.

### Step 2: Core pegging mechanism

#### Inflation-backed MEL/SYM peg

The core pegging mechanism of Melmint is that _through inflating either MEL or SYM, 1 MEL is pegged to_ $$k$$ _ergs (1 DOSC) worth of sym_. In particular, at every block height, either some SYM or some MEL is printed out of thin air and used to buy the other token in the MEL/SYM market on Melswap, always "nudging" the exchange rate closer to 1 MEL = 1 DOSC worth of SYM.

For example, consider a Melswap market with the following exchange rates:

* $$1$$ MEL = $$2$$ SYM
* $$1$$ ERG = $$1.5$$ SYM
* $$k=1.5$$

Here, "1 DOSC worth of SYM" would be "1.5 erg worth of SYM", or $$1.5\times 1.5 = 2.25$$ SYM. Yet the current market exchange rate is only $$2$$ SYM, meaning that _mels are too cheap_.

Thus, to support the peg, every block Melmint will print up some SYM to buy up MEL, until the peg holds.

#### What backs MEL?

Any stablecoin can easily hold a peg when the stablecoin is too expensive --- just print more. The true challenge is when the peg must be defended when the market price is below the peg. The stablecoin must in some way be "backed" by another asset, with an independent value, that it can be redeemed for at the pegged value.

In our case, MEL's value is backed by the _value that can be extracted by inflating sym_. We call this value the **implicit reserve** of Melmint. Because inflating SYM is essentially a tax on all holders of SYM, the implicit reserve is very roughly the market capitalization of SYM. We can therefore say that **SYM backs MEL**.

Thus, to be stable in a worst-case scenario where everybody wants to sell their mels, the **MEL marketcap must stay below the SYM marketcap**. Fortunately, as the [original Melmint paper shows](https://docs.themelio.org/assets/mel.pdf) that's likely to be the case in any reasonable economic conditions.

## Failure scenarios

### Drastic changes in DOSC purchasing power

Because Melmint pegs 1 MEL to 1 DOSC, if the value of a DOSC drastically changes, MEL will lose its stable purchasing power. Historically, this has not really happened, and due to the definition of a DOSC in terms of _time_ rather than _amount_ of computation, usual technological improvements will not cause shocks to the value of a DOSC. Instead, an external shock must greatly affect _how expensive is running the fastest processor available_. Here are some hypothetical scenarios where the value of a DOSC will drastically change:

**Sudden increases in DOSC value**:

* Massive energy crisis makes electricity 10x more expensive than before.
* Somebody finds a way to build an extremely expensive, energy-inefficient machine that does sequential computation 10x faster than top-end machines, at 100x the daily cost.

**Sudden decreases in DOSC value**:

* Breakthrough in energy generation makes electricity nearly free
* Breakthrough in processor design leads to the domination of extremely high core-count machines where the per-core operational cost is drastically lower, yet sequential speed is comparable to current processors
  * "GPU except every core is a fully general-purpose CPU"

### Sudden decrease in Mel market sentiment

For a variety of reasons (say, a general cryptocurrency crash), there might be extremely rare scenarios where a large amount of SYM or MEL is simultaneously panic-sold. This threatens the basis of the Melmint peg by reducing both MEL demand and its implicit reserve, and in a case where MEL issuance cannot be backed by the implicit reserve, the peg may no longer be tenable.

The danger here is hyperinflation of SYM and subsequent mechanism collapse, as Melmint desperately prints SYM to buy and prop up the value of MEL, until both SYM and MEL are utterly worthless. Fortunately, this is inherently prevented by the way Melmint works --- per the Melmint specification, the amount of assets printed to support the peg is _proportional to existing liquidity in the SYM/MEL market_. This means that "how fast" Melmint reacts depends on market conditions. In a panic where market participants anticipate a possible Melmint-driven SYM hyperinflation that will cause both MEL and SYM to become worthless, liquidity will quickly drain from the MEL/SYM market as both MEL and SYM are dumped for alternative assets, making the prophecy self-refuting.

Without MEL/SYM liquidity on Melswap, Melmint is effectively turned off. **The peg will fail**, massively increasing MEL volatility, but a wholesale monetary collapse will be averted. This is especially because unlike users of USD stablecoins, MEL users never expected zero exchange-rate risk and are unlikely to completely dump MEL due to a temporary pause of the peg. The economic impact of such a depeg is likely to be around the same order of magnitude as a fiat "currency crisis" --- bad but not catastrophic.

Once sufficient liquidity returns to the Melswap MEL/SYM market, the peg will gradually be restored.
