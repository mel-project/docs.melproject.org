---
description: >-
  A step-by-step guide to transferring Themelio coins to Ethereum using
  FancyName via our client implementation, fancyname-cli.
---

# Bridge your coins

To safely and conveniently streamline interaction with FancyName, a client has been implemented in Rust which abstracts away the complexity of querying the chain state and crafting compliant transactions on both networks. It is packaged as a simple-to-use CLI application whose source code can be viewed [here](https://github.com/themeliolabs/bridge-cli).

## Moving Themelio coins to Ethereum

In order to mint your Themelio coins as tokens on Ethereum, you must first lock your coins up on the Themelio network and then send a proof of the lock transaction to the Ethereum network. `fancyname-cli` takes care of abstracting the entire process away using an interactive CLI session which will guide you through the bridging process, one step at a time.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ fancyname-cli to-ethereum -t &#x3C;themelio-wallet-name> -e &#x3C;ethereum-wallet-dir>
</strong>
Welcome to fancyname-cli! I'll help guide you through the bridging process.

First, what value and denomination of tokens do you want to bridge to Ethereum?
<strong>14000.0 SYM
</strong>
You are choosing to bridge 14000.0 SYM from Themelio to Ethereum.
The total Ethereum fees will be 0.07273046 ETH.
Do you wish to proceed? (y/n):
<strong>y
</strong>
Bridging your coin, this process may take a couple of minutes.
................................
Success! Your tokens were successfully minted with transaction hash 0xc4408a86465c500c8eea7ded6b2c3c1d9d505470f6587473ca1342073c00d676
</code></pre>

## Moving Ethereum tokens back to Themelio

{% hint style="info" %}
**Note**: FancyName currently supports moving Themelio coins to Ethereum and back, but not the other way around (i.e. it can move wrapped MEL tokens from Ethereum back to Themelio, but it cannot move ETH to Themelio).
{% endhint %}

To move your assets from Ethereum back to Themelio, you must choose a coin to unlock, burn the equivalent amount on the Ethereum network, and provide the Themelio network with a proof of this burn. To automate this process, we can use an interactive `fancyname-cli` session which will safely walk you through the process.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ fancyname-cli to-themelio -e &#x3C;ethereum-wallet-dir> -t &#x3C;themelio-wallet-name>
</strong>
Welcome to fancyname-cli! I'll help guide you through the bridging process.

First, let me query FancyName for locked coins. This might take a few seconds.
....
Okay, choose one of the following locked coins to unlock:
(1) 14000.0 SYM
(2) 34343434343.34343 MEL
(3) 3.14 CUSTOM-ab18c283da41e5cd87e91b5f142e7f4da5c29014c3568140a25e1fe67bceb6c6
Choose between coins 1-3:
<strong>1
</strong>
You are choosing to bridge 14000.0 SYM from Ethereum to Themelio.
The total Themelio fees will be 12.273046 MEL.
Do you wish to proceed? (y/n):
<strong>y
</strong>
Bridging your tokens, this process may take a couple of minutes.
...................
Success! Your coin was successfully unlocked with transaction hash 0x4262932c3c65a83ad1d4bcbcb3aef906c76cd9eaa3fb98b5fde7d0ccc88e089a
</code></pre>

As we can see, the bridging process to and from Themelio has been made as convenient as possible for end users. Now that we know how to interact with FancyName using `fancyname-cli`, let's take a deeper look at how everything actually works in the next section.
