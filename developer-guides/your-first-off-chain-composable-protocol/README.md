# Your first off-chain composable protocol

_Briefly_ describe the paradigm

* Old "on chain composability" model:&#x20;
  * protocol's consumers are _on-chain_ entities like accounts, other contracts, etc
  * integrating with other on-chain protocols: :relaxed:
  * integrating with anything off-chain or cross-chain: :angry::exploding\_head::angry:
* New "off chain composability" model:
  * protocol's consumers are _off-chain_ protocols, apps, etc.
    * filesharing apps
    * websites
    * games
    * etc
  * easily gives blockchain-backed secure decentralization to off-chain systems

Describe the protocol we're building: a trivial _naming system_ that binds random names like `0xdeadbeef` to arbitrary user-controlled data.

Objectives:

* Main security property: **identity retention**
* Product: a Rust library for name registration and lookup that any Rust program can easily integrate
  * Must be both efficient and trustless

