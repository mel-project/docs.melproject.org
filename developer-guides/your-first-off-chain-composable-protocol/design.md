# Design

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
