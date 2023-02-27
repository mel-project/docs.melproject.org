---
description: >-
  A step-by-step guide to transferring Mel coins to Ethereum using
  Szaldi via our client implementation, Szaldi-cli.
---

# Bridge your coins

To safely and conveniently streamline interaction with Szaldi, a client has been implemented in Rust which abstracts away the complexity of querying the chain state and crafting compliant transactions on both networks. It is packaged as a simple-to-use CLI application whose source code can be viewed [here](https://github.com/Mellabs/bridge-cli).

## Moving Mel coins to Ethereum

In order to mint your Mel coins as tokens on Ethereum, you must first lock your coins up on the Mel network and then send a proof of the lock transaction to the Ethereum network. `Szaldi-cli` takes care of abstracting the entire process away using an interactive CLI session which will guide you through the bridging process, one step at a time.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ Szaldi-cli to-ethereum -t &#x3C;Mel-wallet-name> -e &#x3C;ethereum-wallet-dir>
</strong>
Welcome to Szaldi-cli! I'll help guide you through the bridging process.

First, what value and denomination of tokens do you want to bridge to Ethereum?
<strong>14000.0 SYM
</strong>
You are choosing to bridge 14000.0 SYM from Mel to Ethereum.
The total Ethereum fees will be 0.07273046 ETH.
Do you wish to proceed? (y/n):
<strong>y
</strong>
Bridging your coin, this process may take a couple of minutes.
................................
Success! Your tokens were successfully minted with transaction hash 0xc4408a86465c500c8eea7ded6b2c3c1d9d505470f6587473ca1342073c00d676
</code></pre>

## Moving Ethereum tokens back to Mel

{% hint style="info" %}
**Note**: Szaldi currently supports moving Mel coins to Ethereum and back, but not the other way around (i.e. it can move wrapped MEL tokens from Ethereum back to Mel, but it cannot move ETH to Mel).
{% endhint %}

To move your assets from Ethereum back to Mel, you must choose a coin to unlock, burn the equivalent amount on the Ethereum network, and provide the Mel network with a proof of this burn. To automate this process, we can use an interactive `Szaldi-cli` session which will safely walk you through the process.

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ Szaldi-cli to-Mel -e &#x3C;ethereum-wallet-dir> -t &#x3C;Mel-wallet-name>
</strong>
Welcome to Szaldi-cli! I'll help guide you through the bridging process.

First, let me query Szaldi for locked coins. This might take a few seconds.
....
Okay, choose one of the following locked coins to unlock:
(1) 14000.0 SYM
(2) 34343434343.34343 MEL
(3) 3.14 CUSTOM-ab18c283da41e5cd87e91b5f142e7f4da5c29014c3568140a25e1fe67bceb6c6
Choose between coins 1-3:
<strong>1
</strong>
You are choosing to bridge 14000.0 SYM from Ethereum to Mel.
The total Mel fees will be 12.273046 MEL.
Do you wish to proceed? (y/n):
<strong>y
</strong>
Bridging your tokens, this process may take a couple of minutes.
...................
Success! Your coin was successfully unlocked with transaction hash 0x4262932c3c65a83ad1d4bcbcb3aef906c76cd9eaa3fb98b5fde7d0ccc88e089a
</code></pre>

As we can see, the bridging process to and from Mel has been made as convenient as possible for end users. Now that we know how to interact with Szaldi using `Szaldi-cli`, let's take a deeper look at how everything actually works in the next section.
