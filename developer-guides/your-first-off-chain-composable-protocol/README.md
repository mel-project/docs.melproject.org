# Your first off-chain composable protocol

_Briefly describe the paradigm_

* _Old "on chain composability" model:_&#x20;
  * _protocol's consumers are on-chain entities like accounts, other contracts, etc_
  * _integrating with other on-chain protocols:_ :relaxed:__
  * _integrating with anything off-chain or cross-chain:_ :angry:__:exploding\_head:__:angry:__
* _New "off chain composability" model:_
  * _protocol's consumers are off-chain protocols, apps, etc._
    * _filesharing apps_
    * _websites_
    * _games_
    * _etc_
  * _easily gives blockchain-backed secure decentralization to off-chain systems_

## On-chain vs. off-chain composability

Almost all legacy blockchains fall into the trap of favoring on-chain composability, where all of the protocol's consumers are native entities like EOA's and smart contracts. This works well for integrating with other on-chain protocols but causes extreme difficulty with off-chain and cross-chain integrations, perpetuating a web3 walled garden which other decentralization and security-focused applications are unable to leverage. This is where Themelio comes in.

Themelio's vision is built on the _off-chain_ composability paradigm. This paradigm centers off-chain consumers; filesharing apps, websites, privacy protocols, games, etc. are all first class citizens on Themelio. This off-chain compatibility is what gives Themelio its competitive edge: _all_ applications can now inherit the decentralization and security properties of blockchains, with ease.



_Describe the protocol we're building: a trivial naming system that binds random names like `0xdeadbeef` to arbitrary user-controlled data._

_Objectives:_

* _Main security property: **identity retention**_
  * _What the name is bound to cannot be changed without the consent of its current owner_
* _Product: a Rust library for name registration and lookup that any Rust program can easily integrate_
  * _Must be both efficient and trustless_
* _Why not another blockchain??_
  * _Deal-breaker: almost impossible to look up names trustlessly on-chain_
  * _Easy to look up on chain, but humans are off-chain_
  * _Crossing that boundary usually involves sacrificing decentralization/security_

## Your first off-chain composable app

To better illustrate the concept of off-chain composability, we will be using Themelio to build a trivial naming system that binds random names, like `0xdeadbeef`, to arbitrary, user-controlled data. The main security property of our application will be **identity retention**, i.e., name-to-data mappings which cannot be changed without the consent of their current owner.

To this end we will create our app as a Rust library which abstracts away the name registration and lookup process so that any other Rust project can easily integrate its functionality. The main criteria for our library are that it should be both efficient and trustless.

## Why Themelio and not some other L1?

While it is relatively easy to create a similar application on an alternative L1 blockchain, it will only be able to be queried by other on-chain entities, while integration with any off-chain or cross-chain applications would be essentially impossible without sacrificing either decentralization or security.

