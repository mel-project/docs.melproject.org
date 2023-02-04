# Next steps

{% hint style="info" %}
* Recap of what Gibbername allows us to do
  * Register, transfer, and look up names from any Rust program, without clunky dependencies
  * Totally trustless, no RPC network or full node is trusted!
* How would a production naming system work?
  * Moving things even more off-chain
    * Only have the hash on-chain
      * Use a P2P network (e.g. a DHT) to store the actual name binding
      * Lets us also get rid of the -1 state spam, as long as we can sacrifice proofs of nonexistence
  * Binding human-readable names
    * Use a tree structure.
    * A little complicated, for another day
  * Better wallet integration UX
    * Graphical frontends and graphical wallets: a much nicer workflow
{% endhint %}
