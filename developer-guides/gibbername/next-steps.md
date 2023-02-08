# Next steps

## What did we just do?

We just finished building **Gibbername**, a simple on-chain naming system. We built a library and simple command-line tool that lets you register, transfer, and look up "gibbernames" like `goltor-rulnuq-keh` from any Rust program.

Most importantly, this whole process is entirely trustless! A malicious RPC network or full node is unable to lie to a Gibbername client at all.

Hopefully, you also got a taste of how different Mel's off-chain composability is from the current Web3 paradigm. Instead of writing a smart contract with an on-chain API that other on-chain code calls, you designed an on-chain data structure that _off-chain_ code maintains and looks up. This then enables you to write apps that integrate Gibbername's decentralized security without writing a single line of on-chain smart contract code (something that you can't really do with ENS and the like!)

## Possible improvements

Gibbername, as it is, is close to the simplest possible useful protocol we can build on Mel. An actual, production naming system that could compete with DNS and the like would need a few more features. Here we sketch how they can be implemented:

### Off-chain data storage

Right now, the entire binding is stored on the Mel blockchain. This is not all that efficient, since blockchain space is pretty expensive. Furthermore, it can be slow to follow the very long Catena chains that would result from a name that has been rebound many different times.

We can move more data off-chain to solve both problems:

* On-chain, we bind to the _hash_ of the binding rather than the binding itself.
* Off-chain, we use an insecure decentralized key-value mapping, like a DHT, to map each gibbername to a _proof of on-chain binding_, consisting of three items:
  * The initial on-chain transaction, as well as a Merkle _proof of inclusion_ that the blockchain location represented by the gibbername truly maps to this transaction
  * The _last_ on-chain transaction in the Catena chain, along with a proof that the `Denom::Custom(...)` output is unspent.
    * We do not need any transactions in between, since there could not possibly be another unspent output with the same Denom, since there's only 1 in total produced by the first transaction.
  * The contents of the binding itself, which must hash to the on-chain binding in the output of the last on-chain transaction.

In such a system, lookups can _entirely be done off-chain_ and massively scale. Bindings can also be arbitrarily large without burdening the blockchain.

### Human-readable names



### Better registration UX



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
