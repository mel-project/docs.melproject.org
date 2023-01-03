# Swapping Coins

## Prerequisites

In [the last section](getting-started.md) we sent `bob` some MEL. In this section we are going to use that wallet. To follow along make sure you have access to at least 500 MEL.

{% hint style="info" %}
Fees, exchange rates, and other numbers are **entirely fictional**!
{% endhint %}

<pre class="language-shell-session"><code class="lang-shell-session">$ melwallet-cli summary -w bob
<strong>
</strong>Network:      testnet
Address:      t04ncd9j314rt7jth5wmedz8j5tcz9w8cdcdk48ex9t2fkj44ekne0
Balance:      500.000000  MEL
              500.000000  MEL
Staked:       0.000000    SYM
</code></pre>

## Swapping coins <a href="#swapping-coins" id="swapping-coins"></a>

Unlike other blockchains where this functionality is typically on a programmable smart contract, Themelio features a built-in, Uniswap-like decentralized exchange (DEX) called **Melswap**.

{% hint style="info" %}
We build a rudimentary DEX into the L1 not primarily for convenience, but as a trustless price oracle for designing on-chain logic, as well as an important part of the Melmint algorithm that stabilizes MEL works.

Nevertheless, you'll see that it's very easy to use!
{% endhint %}

With Melswap, any user can instantly swap one coin and another for a fixed pool fee of 0.5%. Using `melwallet-cli` we will swap 100 `MEL` for some `SYM` at the market rate.

<pre class="language-shell-session"><code class="lang-shell-session">$ melwallet-cli swap -w bob --from MEL --to SYM 100.0
<strong>
</strong>SWAPPING
From:  100.000000 MEL
To:    50.000000  SYM  (approximate)
Fee:     0.000000 MEL
Proceed? [y/N] y

Transaction hash:  ...
Awaiting Confirmation... Confirmed at height 314!
Block Explorer: https://scan-testnet.themelio.org/blocks/314/...
</code></pre>

As you can see in the `From` and `To` fields, 100 `MEL` is being swapped for 50 `SYM`. After user confirmation, `melwallet-cli` waits for the transaction to be posted to the blockchain then outputs the confirmation height and [melscan](https://scan.themelio.org) URL.

<pre class="language-shell-session"><code class="lang-shell-session">$ melwallet-cli summary -w bob
<strong>
</strong>Wallet name:  bob (unlocked)
Network:      custom02
Address:      t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g
Balance:      400.00000   MEL
              50.000000   SYM
Staked:       0.000000    SYM
</code></pre>

### Pools

In order to execute the trade above, we interacted with a **liquidity pool**: a collections of two kinds of assets, in this case `MEL` and `SYM`, deposited on-chain. \*\*\*\* Liquidity pools provide constant-product DEXes like Melswap with always-available buyers and sellers, with an exchange rate that automatically adjusts to satisfy any trade without running out of assets in the pool.

{% hint style="info" %}
Constant-product swapping pools were invented by Uniswap, and the Uniswap v2 documentation remains the best guide to understanding them further.
{% endhint %}

To see the current exchange rate and liquidity of any given pool use the `melswap-info` subcommand

<pre><code>melwallet-cli melswap-info --pool MEL/SYM
<strong>
</strong>1 MEL  = 0.5000000000000000 SYM
1 SYM  = 2.0000000000000000 MEL
Liquidity tokens issued: 100.000000 MEL~SYM
</code></pre>

{% hint style="warning" %}
A constant-product pool like Melswap works to ensure all trades can be satisfied and as such extremely large trades will receive **extremely** bad prices to preserve pool liquidity.

(TODO: Example)
{% endhint %}

### Providing Liquidity

Where does all the liquidity sitting in the pool come from? Melswap incentivizes users to add liquidity to pools by depositing tokens in the pool in exchange for **liquidity tokens**. Liquidity tokens are unique in that they reflect an ownership of a proportion the entire pool, instead of an individual token. Let's deposit a total value of 100 `MEL`in liquidity into the `MEL/SYM` pool:

<pre data-overflow="wrap"><code>melwallet-cli liq-deposit -w bob --pool MEL/SYM --value 100 MEL
<strong>
</strong>TRANSACTION RECIPIENTS

Reciever Addresses
t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g (self) 1.000000 MEL~SYM  ""

Senders Addresses                                             Amount          Additional data
t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g (self) 50.000000 MEL  ""
t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g (self) 25.000000 SYM  (2:1) ""
(network fees)                                                0.0000000 MEL

Proceed? [y/N] y
Transaction hash:  b02ed062fc8f9eb6b2fce799e36ad06b86b95a45ef3bd8b98d2ee8a7deae0691
Awaiting Confirmation... Confirmed at height 316!
Block Explorer: https://scan-testnet.themelio.org/blocks/314/b02ed062fc8f9eb6b2fce799e36ad06b86b95a45ef3bd8b98d2ee8a7deae0691
</code></pre>

As you can see bob now has 1 liquidity token called `MEL~SYM`

<pre class="language-shell-session"><code class="lang-shell-session">$ melwallet-cli summary -w bob
<strong>
</strong>Wallet name:  bob (unlocked)
Network:      custom02
Address:      t11n9ynz8jhcd1k7h6jx4pvsc4m2qvwjhdp5mx03ega05hkcts8j9g
Balance:      349.50000   MEL
              25.000000   SYM
              1.0000000   MEL~SYM
Staked:       0.000000    SYM
</code></pre>

{% hint style="danger" %}
Just like traditional markets, on Themelio there are different risks associated with different asset classes. Please do not invest your money in any asset until you understand the risks, and never risk more than you can afford. For more information about Themelio assets please take a look at (TODO)
{% endhint %}



