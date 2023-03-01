# Governance-free neutrality

An important principle of Mel is that _after the protocol stabilizes, governance is not socially legitimate_. This means that once the proper mainnet launches, MEL and SYM are widely circulating, and the protocol reaches "1.0", Mel is not supposed to allow, or need, any sort of changes (e.g. "hard/soft forks") to the core blockchain rules.

Governance-freedom has an interesting _synergy_ with Mel's off-chain composable vision. An off-chain composable ecosystem needs an L1 that does not change, since protocol upgrades tightly couple off-chain app development and L1 improvements, and off-chain ecosystems are unlikely to have a unified "community", making the credible neutrality of their common L1 both important and fragile. In the other direction, a L1 that wants to be governance-free needs to be used extensively by off-chain apps --- an primarily on-chain ecosystem encourages rather than resists governance.

## Off-chain needs governance-freedom

The key property of blockchains is **endogenous trust**: we can trust a blockchain _without trusting any of the people running it_. Trust emerges from internal incentives that drive rational actors, rather than from preexisting trust in the parties that run the protocol. This is probably in fact _only_ found in blockchains and similar tech: BitTorrent has decentralization, Keybase has transparency, and TLS has security, to name a few, but none of them have endogenous trust.

Any sort of consensus-breaking governance (like the numerous Ethereum network upgrades) inherently damages endogenous trust, since it enables _external actors to rewrite the protocol upholding the internal incentives_ --- ultimately rooting trust outside the protocol, in the hands of developers, community sentiment, etc.

Trading away the blockchain killer feature is sometimes "okay" for an on-chain ecosystem; not everything on-chain is there to maximize endogenous trust, and often having your decentralized, transparent "world computer" backed by trusting the Ethereum/Solana/etc community is perfectly fine, and the advantages of the rich composability and interoperability offered by smart contract ecosystems outweigh any reduction in security.

But it is not fine for off-chain apps, whose _only reason_ to use a blockchain is endogenous trust. Removing trust in community-run Tor directory servers, for instance, is a lot less appealing if the only result is shifting trust from the Tor community to the Ethereum community. Only an L1 with an uncompromising focus on the robust credible neutrality and reliability offered by purely incentive-based, endogenous trust can unlock new horizons in decentralized security in the off-chain world.

Furthermore, off-chain ecosystems are far more disrupted by protocol upgrades compared to L1 smart contract ecosystems. Even far-reaching changes to the blockchain rules (e.g. Ethereum's transition to PoS) can be made seamless to on-chain code, but these protocol upgrades entail _every single blockchain client upgrading at once_. In an off-chain ecosystem, blockchain light client implementations will massively proliferate as dependencies of any program that needs to interact with on-chain logic, making coordinating such an upgrade extremely difficult and failures highly disruptive. While a protocol upgrade for current L1s means at worse a glitchy day on cryptocurrency exchanges and wallets, a upgrade of an L1 underlying a global off-chain ecosystem means all sorts of apps greatly removed from the "crypto community" requiring updates, and perhaps even embedded or unmaintained clients simply becoming "bricked".

## Off-chain produces governance-freedom

The interesting other side of the coin is that precisely because protocol upgrades are so disruptive, an off-chain ecosystem _socially enforces L1 governance-freedom_. Coordinating everybody in a dispersed ecosystem, full of diverse protocol stacks and communities, to switch to a different set of protocol rules is practically impossible.

A great example of this dynamic in action is the Internet Protocol. Like Mel in an off-chain composable ecosystem, IP is a deeply embedded, simple, and low-level principle that is crucial for the functioning of many different protocol stacks.

And as we would expect, "governance" on IP is _extremely_ hard. Ever since IPv6 was created in 1998, many influential members of “the community” have spent great efforts to try to replace the older IPv4 with its technically superior successor. Impressive-sounding events sponsored by “big shots”, like the Internet Society’s “World IPv6 Day” in 2011 and “World IPv6 Launch Day” in 2012 were supposed to jump-start IPv6 adoption.

Yet IPv4, though it doesn't even try to be governance-free and in fact has serious limitations like a drastic shortage of IP addresses, is just not going. 25 years later and 11 years after “World IPv6 Launch”, only a little more than 30% of networks support IPv6, and the vast majority of Internet traffic continues to be IPv4.

Thus, even the security of Mel's incentives not changing depends on incentive-based endogenous trust, not preexisting trust in some community. The sort of social-coordination problem that prevents a protocol from upgrading is exactly the sort of incentive-based security that upholds the correct execution of a blockchain rules.
