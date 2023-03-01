# Light clients

**Light clients** in Mel are participants that don't replicate the blockchain history or state. Instead, when they want to query on-chain information, they ask any [full node](network-architecture.md) for that information.

The key thing to remember is that _light clients are trustless_ --- they ask a full node for information, but they do not trust the full node not to lie to them. This is absolutely central to enabling scalable off-chain composability, since most off-chain apps would not want to replicate blockchain history.

Trustless light clients are possible due to two things: extensive **state commitments** that allow a light client to verify proofs of blockchain data with only the latest block header, and a **bootstrapping** mechanism that lets clients retrieve the latest block hash while only trusting the [consensus mechanism](consensus.md).

## State commitments

Every Mel block has a short **header** that commits to the contents of that block and previous history, but also the entire _state_ of the blockchain. The detailed data format can be seen in the TodoYellowPaper, but on a high level, the Mel block header contains the root hashes for the following **sparse Merkle trees** (SMTs) that commit to on-chain state:

- Block number of a previous block -> its hash
- [Unspent coin](data-model.md) IDs (transaction hash and index) -> coin contents and the height at which it was committed
- [Covenant](data-model.md) hash -> number of unspent coins with that covenant
- A list of all _frozen_ [stakes](consensus.md) (whether active or inactive)

SMTs are a key-value mapping structure that has an important property: they can produce succinct proofs, checkable by anyone with the root hash, that a certain key is mapped to a certain value (proof of inclusion), or that a certain key is absent from the mapping (proof of exclusion).

What this means is that when a light client asks a full node, say, to list all the unspent coins locked by a certain covenant, the full node is able to _prove_ to a client the correctness of the response. (In this case, by providing proofs of inclusion for all the unspent coins, as well as a proof of inclusion of the _count_ of the coins, showing that the full node is not hiding any coins)

This is useful beyond simple queries made by e.g. wallets. In fact, we can design [highly flexible _coin graph traversal_ APIs](../developer-guides/gibbername/melprot-a-quick-intro.md) on top of these verified SMT queries, allowing trustless and efficient manipulation of all sorts of on-chain data:

```rust
// Traverse through the first parent, its first parent, etc of 674735b7b7e4163f7404715bd6b8433a8db523c52279ad07e2b4e88a6708d873 indefinitely, until a coinbase transaction is hit
let client = melprot::Client::autoconnect(NetID::Mainnet);
let traversal = client.traverse_back(
   BlockHeight(1901450),
   "674735b7b7e4163f7404715bd6b8433a8db523c52279ad07e2b4e88a6708d873".parse()?,
   |tx| {
      // always go to the first input
      Some(0)
   }
).boxed();
while let Some(next) = traversal.next().await? {
   println!("transaction found: {:?}", next);
}
```

Of course, all this magic is possible only if we assume that the client already knows the latest block header. That requires a secure bootstrapping procedure.

## Bootstrapping light clients

Before doing any queries, a light client must know the latest block header. Of course, the client can't just ask the full node for this info --- the full node could lie and subvert all the security guarantees of Mel!

Instead, the client follows a two-step procedure:

1. The _staker vote weights_ of the current [consensus epoch](consensus.md) are determined, based on the active stakes of the current epoch.
2. The latest block header, together with a _consensus proof_ that more than 2/3 of the vote weight voted for the block header, is retrieved from the full node. This then proves that the claimed block header really was approved by the blockchain consensus.

### Determining the vote weights

Recall that in the [Mel's consensus mechanism](consensus.md), the set of active stakers and their voting weights is the same during every 200,000-block epoch, being determined by which stakes are "active" during this epoch.

It turns out that since the 1. the block header commits to all locked stakes, not just all active ones, 2. stakes must be locked before their first active epoch starts, _the last block header of epoch N commits to the active stakes of epoch N+1_.

This gives us an elegant recursive algorithm to derive the current stake set, as long as the client knows _some_ stake set (and it always does, since the initial stake set is hardcoded):

To get the active stakes of epoch N:

- If we already know it, we are done. (base case)
- Otherwise,

  - We get the active stakes of epoch N-1
  - We ask for the last block header of the epoch N-1, verifying the consensus proof with the active stakes we just obtained
  - We ask for the list of all stakes locked by the end of epoch N-1, verifying it against the commitment in the block header
  - We filter the list for stakes that will be active in epoch N, and return that.

### "Ultraweak subjectivity"

There is a subtle attack on the sort of consensus-participant-updating bootstrapping procedure described. If a light client is very out of date (say, it only knows the initial stake set and not anything subsequent), an attacker can collude with _old stakers who have completely unstaked_ to sign malicious claims about block headers --- including epoch-end block headers --- and completely mislead a client as to the current state of the blockchain.

This problem, called "weak subjectivity", is common to PoS blockchains. It only works on old clients, since incentives ([slashing](consensus.md)) defend against collusion with newer stakers who still have money at stake.
