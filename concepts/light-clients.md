# Light clients

**Light clients** in Mel are participants that don't replicate the blockchain history or state. Instead, when they want to query on-chain information, they ask any [full node](network-architecture.md) for that information.

The key thing to remember is that _light clients are trustless_ --- they ask a full node for information, but they do not trust the full node not to lie to them.
