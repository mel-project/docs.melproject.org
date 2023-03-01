# What belongs on-chain?

Instead of stuffing as much functionality as possible into on-chain smart contracts, Mel's paradigm encourages using on-chain logic and data much more sparingly, with complex app logic and ecosystem composability happening off-chain. What exactly belongs on-chain then?&#x20;

The answer is that for any decentralized app or protocol, the **minimal root of trust** should be implemented on the blockchain. This means

* the _smallest_ part of the system
* on whose security the whole system's security depends upon

For example, consider an end-to-end encrypted chat app. Most parts of the app aren't actually security-critical. For instance, it isn't particularly important how secure the storage of in-transit messages are, since it's all encrypted anyway --- they could very well be put on some centralized cloud like AWS. But one part of the system is really important: the _public key infrastructure_ (PKI), or the system that lets users know the public keys of other users. If this were centralized and insecure, end-to-end encryption could be entirely defeated through impersonation and [man-in-the-middle](https://en.m.wikipedia.org/wiki/Man-in-the-middle\_attack) attacks.

Thus, the PKI should be built with on-chain trust, either through custom on-chain logic or by leveraging some existing Mel-based protocol (e.g. an ENS-like secure naming system?)

{% hint style="info" %}
More detailed advice on how to practically design the on-chain pieces of an off-chain composable protocol can be found in the [Gibbername tutorial](../developer-guides/gibbername/design.md).&#x20;
{% endhint %}
