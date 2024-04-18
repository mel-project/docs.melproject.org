---
description: A guide on swapping tokens on Mel.
---

# Swapping tokens

## Prerequisites

In [the last section](getting-started.md) we sent wallet `bob` some MEL. In this section we'll use wallet `bob` to swap some tokens. Make sure you have access to at least 500 MEL.

{% hint style="info" %}
Fees, exchange rates, and other numbers in this guide are **entirely fictional**!
{% endhint %}

To see how much MEL `bob` has:
<pre class="language-shell-session"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json summary
</strong>
Network:  testnet
Address:  t7v9tegt6bm6dv9t6e56ktdap3ych5htw83wa69z0shwa7nt3xbkn0
Balances:
500.00000 MEL
</code></pre>

## Swapping tokens <a href="#swapping-coins" id="swapping-coins"></a>

Unlike other blockchains where this functionality typically exists in a programmable smart contract, Mel features a built-in, Uniswap-like decentralized exchange (DEX) called **Melswap**.

{% hint style="info" %}
We embedded a rudimentary DEX into the L1 not primarily for convenience, but as a trustless price oracle for designing on-chain logic; it is also an important component of the Melmint algorithm that stabilizes MEL.
{% endhint %}

With Melswap, any user can instantly swap one token for another for a fixed pool fee of 0.5%. Using `melwallet-cli`, we swap 100 MEL for some SYM at the market rate:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json swap --value 100.0 --from MEL --to SYM --wait
</strong>
SWAPPING
From:  100.000000 MEL
To:    50.000000  SYM  (approximate)
Fee:     0.000000 MEL
Proceed? [y/N] y

..................

Transaction 3717d9d6a93f2e5c4c420745dccbb72d9ed109285362f6a45785e6f73cd8ef58 confirmed!
</code></pre>

<pre class="language-shell-session"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json summary
</strong>
Network:  testnet
Address:  t7v9tegt6bm6dv9t6e56ktdap3ych5htw83wa69z0shwa7nt3xbkn0
Balances:
400.00000 MEL
100.00000 SYM
</code></pre>

### Pools

In the above trade, we interacted with a **liquidity pool**: a collections of two kinds of assets, in this case MEL and SYM, deposited on-chain. Liquidity pools provide constant-product DEXes, like Melswap, with always-available buyers and sellers, via an exchange rate that automatically adjusts to satisfy any trade without running out of assets in the pool.

{% hint style="info" %}
Constant-product swapping pools are most notably implemented by Uniswap, and the [Uniswap v2 documentation](https://docs.uniswap.org/contracts/v2/concepts/protocol-overview/how-uniswap-works) remains the best guide to understanding them further.
{% endhint %}

To see the current exchange rate and liquidity of any given pool:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json pool MEL/SYM
</strong>
1 MEL  = 0.5000000000000000 SYM
1 SYM  = 2.0000000000000000 MEL
</code></pre>

{% hint style="warning" %}
A constant-product pool like Melswap works to ensure all trades can be satisfied. This means that trades without sufficient liquidity available may receive **extremely** bad prices.
{% endhint %}

### Providing liquidity

Where does all the liquidity sitting in the pool come from? Melswap incentivizes users to add liquidity to pools by depositing tokens in the pool in exchange for **liquidity tokens**. Liquidity tokens are unique in that they reflect an ownership of a proportion of the entire pool, instead of an individual token. Let's deposit a total value of 100 MEL in liquidity into the MEL/SYM pool:

<pre class="language-shell-session" data-overflow="wrap"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json liq-deposit 0.0 SYM 100.0 MEL --wait
</strong>
Proceed? [y/N] y
..................
Transaction 19b882d37f66cce058dea501c2acffe1cdac8b00e7424e27ad92c864f27ba56d confirmed!
</code></pre>

`bob` now has 1 liquidity token for the MEL/SYM pool, called `CUSTOM-526bd177e93a854e08216aff6b48cf7f3ae7b4cbd202fdfde39271d1ad90a3bd`.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>melwallet-cli --wallet-path ./bob.json summary
</strong>
Network:      testnet
Address:      t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g
Balance:      349.50000   MEL
              25.000000   SYM
              1.0000000   CUSTOM-526bd177e93a854e08216aff6b48cf7f3ae7b4cbd202fdfde39271d1ad90a3bd
Staked:       0.000000    SYM
</code></pre>
