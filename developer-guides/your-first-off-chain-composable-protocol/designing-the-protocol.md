---
description: Guidance on the initial design of an off-chain, composable protocol.
---

# Designing the protocol

{% hint style="info" %}
How do you typically design an OCC protocol?

* Figure out what, if anything, needs to be on the blockchain
  * Eventually, you can do most things by using existing OCC protocols
  * But not yet :)
* Figure out how to _encode_ that data into the UTXO graph in a way that's easy to verify/lookup by thin clients
  * Common tricks: self-propagation, "NFTs", provably unspendable coins as "pointers" to transactions, encoding mappings as trees
* Then, design the library for interacting with this UTXO graph
  * Don't expose blockchain concepts like "transaction" and "block" unless absolutely necessary (usually, this is only necessary on the "write" side)
  * Never return non-validated information

Follow the steps for the naming protocol

* What needs to be on the blockchain?
  * Something that you can overwrite / move, given certain permissions
  * This maps pretty nicely to asset ownership
* How do you encode this?
  * An "NFT" (token with total supply 1 microunit)
    * The unique token `Denom` is the name of the name
  * The `additional_data` of the only unspent coin with this Denom
  * This lets you prove that some name is bound to this data, while having trustless identity retention
  * **But**: you cannot _look up_ names with the `melprot::Client` API
  * Fix: token with _2_ microunit supply. Initial transaction has two outputs, the second of which is sent to a provably unspendable covenant hash (`0xaaaa....`)
    * `txhash-1` can always be looked up to find the original registration info
    * We can then binary-search in the history to find each subsequent name transfer, eventually finding the current binding.
{% endhint %}

_Draw a picture. Talk about how you typically design a low-level protocol like this: think about the coingraph structure!_



## Intuition for designing your own off-chain, composable protocol

Building an off-chain, composable (OCC) protocol isn't like building a typical SaaS app. It's helpful to think about what _needs_ to go on the blockchain and what can safely be done off-chain. Here, it's helpful to treat the blockchain as something analogous to a billboard: there is limited space to display what you want, and it's probably not cheap to do so. Similarly, a blockchain is relatively costly to store data on, so, when building an OCC protocol, you should consider the question: "What data absolutely needs to go on the blockchain?". Another way to phrase this is: "What data needs to be globally consistent and transparent to everyone?"

Eventually, as the Themelio ecosystem expands and matures, most applications will be able to be built by leveraging existing OCC protocols, in a way similar to the "money legos" paradigm present in current, on-chain defi ecosystems. This means that eventually, integrating blockchain security, decentralization and censorship resistance into an off-chain app will be as easy as integrating a library into your project; all blockchain interactions and RPC calls will be abstracted away similarly to how low-level protocols like TCP/IP are today.

## Encoding critical data into the UTXO graph

Now that we've decided on what application critical data must be stored on-chain, we must figure out how to _encode_ that data into the on-chain UTXO graph in a way that's easy to verify and look up by thin clients.

A few "common tricks" for doing this include:

* self-propagation (i.e. the covenant for each UTXO in this graph requires that all descendants have the same covenant)
* "NFTs" (coins with unique denominations and a supply as small as 1)
* provably unspendable coins as "pointers" to transactions (e.g., a coin tied to a mathematically impossible covenant hash, such as `0x00...00`)
* encoding mappings as trees (Merkle trees, tries, etc.)
