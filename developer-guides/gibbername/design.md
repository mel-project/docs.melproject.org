# Design

{% hint style="info" %}
How do you typically design an OCC protocol?

- Figure out what, if anything, needs to be on the blockchain
  - Eventually, you can do most things by using existing OCC protocols
  - But not yet :)
- Figure out how to _encode_ that data into the UTXO graph in a way that's easy to verify/lookup by thin clients
  - Common tricks: self-propagation, "NFTs", provably unspendable coins as "pointers" to transactions, encoding mappings as trees
- Then, design the library for interacting with this UTXO graph
  - Don't expose blockchain concepts like "transaction" and "block" unless absolutely necessary (usually, this is only necessary on the "write" side)
  - Never return non-validated information

Follow the steps for the naming protocol

- What needs to be on the blockchain?
  - Something that you can overwrite / move, given certain permissions
  - This maps pretty nicely to asset ownership
- How do you encode this?
  - An "NFT" (token with total supply 1 microunit)
    - The unique token `Denom` is the name of the name
  - The `additional_data` of the only unspent coin with this Denom
  - This lets you prove that some name is bound to this data, while having trustless identity retention
  - **But**: you cannot _look up_ names with the `melprot::Client` API
  - Fix: token with _2_ microunit supply. Initial transaction has two outputs, the second of which is sent to a provably unspendable covenant hash (`0xaaaa....`)
    _ `txhash-1` can always be looked up to find the original registration info
    _ We can then binary-search in the history to find each subsequent name transfer, eventually finding the current binding.
    {% endhint %}

## Overall design process

Designing a off-chain composable, Mel-backed protocol is roughly a three-step process:

1. **What needs to be on the blockchain?** In the mature off-chain composability utopia of the future, most of the time you don't need to put anything on the blockchain yourself. For example, given an anonymous communication network and a secure naming system, an I2P-like anonymous web hosting platform can be built by combining the two protocols. But for the low-level primitives we need to build right now (like Gibbername), generally something needs to be on-chain.
2. **How to encode the on-chain info in a _thin-client legible_ fashion?** We need to then figure out how to encode the on-chain info in the on-chain coin graph in a way that, given the `melprot` data model, can easily and trustlessly be queried by off-chain programs.
3. **How to uphold invariants in the on-chain data?** Often, we need to force the on-chain data to be of a certain shape in order for our encoding to work. This generally requires either writing Melodeon covenants or exploiting some trick of Mel's coin model.

Let's follow this process for Gibbername!

## Designing Gibbername

### What needs to be on the blockchain?

We don't have any sort of off-chain decentralized key-value store available. So we need to encode the whole contents of the naming system on-chain.

More specifically, for every gibbername, we need to somehow store an on-chain blob of data. Since the blockchain is immutable, we really want to store the _history_ of a particular gibbername on-chain. Anybody can then look up this history to get the data bound to a gibbername.

### How to encode the on-chain info?

There is a standard technique for encoding append-only histories in a coin graph: a **Catena chain** (TODO LINK). A Catena chain is simply a chain of transactions, each one spending a particular output coin (say, the first) of the previous one. Metadata on the coins or transactions then encodes the append-only log, and the transaction hash of the first element uniquely identifies the whole chain.

```
a picture of a first-output catena chain, probably lifted from literature
```

In our case, we can use the `CoinData::additional_data` field of the coins within a Catena chain to record the binding history of a gibbername. The gibbername itself (like `hehheh-hehheh`) would be some form of encoding of the _location of the first link in the chain_,

Now, thin clients are able to do the core Gibbername features:

- **Lookup**: The latest, unspent entry in the chain will contain the latest piece of data bound to that gibbername.
- **Bind**: To bind the name to a different piece of data, a new item can simply be added at the end of the Catena chain.
- **Transfer**: Whoever can spend the last item of the Catena chain "owns" the name and has exclusive access to rebind the name or transfer it to a different owner. Rebindings are just a special kind of transfer that repeats the same address as the last binding. (The "permission" is specified in the covenant hash, or "address", embedded in the last coin)

### How to uphold invariants?

How do we ensure that the owner of a Gibbername actually continues the Catena chain? We _could_ just define the Catena chain as always continuing from the first output and place no constraints at all --- this will give us a canonical interpretation of _any_ transaction as a gibbername and its subsequent first child, first grandchild, etc as a Gibbername binding history.

But this has several disadvantages:

- It doesn't mark Gibbername activity out in the blockchain, making it easy to mistake other transactions as gibbernames
- Using any regular chain of coins makes it very easy to accidentally rebind or transfer a gibbername. Wallet software would not be able to distinguish Gibbername coins from regular $MEL coins, and would accidentally spend the first and mess up the binding without a lot of manual intervention.

Instead, we use **special transaction metadata** **custom token denomination** to mark Catena chains used by Gibbername. In particular: the _first_ transaction in a Gibbercoin Catena chain must:

- Have the `Transaction::data` field set to `"gibbername-v1"`
- Have _one of its outputs_ have denomination `Denom::NewToken` and value `1`.

Subsequently, the canonical Catena chain is defined as the unique chain of coins that have denomination `Denom::Custom(<transaction hash of the first transaction>)`.

This exploits two nice features of Mel's transaction model:

- A coin with `Denom::NewToken` creates a new, unique token denomination named after the hash of its parent transaction.
- A coin with value `1` can no longer be subdivided by spending transactions. There's thus always only going to be one unspent coin in the world with the right denomination, making a unique Catena chain.

```
picture illustrating the result
```

Gibbername transactions are now very obvious (allowing, say, Melscan to offer a global Gibbername listing), and wallets will no longer accidentally spend Gibbername-related coins because they have their own denomination.
