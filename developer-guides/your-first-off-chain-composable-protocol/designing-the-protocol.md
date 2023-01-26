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
{% endhint %}

Draw a picture. Talk about how you typically design a low-level protocol like this: think about the coingraph structure!
