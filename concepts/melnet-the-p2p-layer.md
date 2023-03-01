# Melnet: the P2P layer

Mel uses a very simple P2P protocol, melnet, for communication among [full nodes](network-architecture.md) and between full nodes and light clients. Implemented as a [Rust crate](https://crates.io/crates/melnet2) within the [nanorpc](https://crates.io/crates/nanorpc) framework, it has the following features:

* Unstructured gossip network, similar to Bitcoin (and unlike libp2p and similar DHTs)
* Uses JSON-RPC 2.0 over plain HTTP/1.1. Not even WebSockets is used.

Using simple web standards makes it easy for (still future) web-based clients written in JavaScript or WebAssembly to directly access the P2P network. In-browser light clients, or even full nodes, become possible.

{% hint style="info" %}
Eventually, we'll have a more detailed description and spec of melnet here. For now, [check out the actual codebase on GitHub](https://github.com/mel-project/melnet2)!
{% endhint %}
