---
description: Learn how melnodes maintain global consensus across the network
---

# Consensus

## Can we all just agree on something?!

At the bottom of it all, a blockchain is a distributed network of nodes that all agree on some sequence of events. If you look past all the tokens and proofs, you'll find that almost all distributed systems have to solve pretty much the same problem of agreeing on the same thing.

## How do melnodes agree on things?

The problem of consensus has been studied and researched for quite a while already, so there are lots of options to choose from. Our choices range from complex protocols like Paxos and PBFT, slightly less complex (but still complex) ones like Raft, to newer and simpler ones like Streamlet.&#x20;

To your genuine surprise :exploding\_head:, Mel went with a slightly modified version of Streamlet, called Streamlette.

## How does Streamlette work



## Resources

* [DecentralizedThoughts blog post](https://decentralizedthoughts.github.io/2020-05-14-streamlet/)
* A [video](https://www.youtube.com/watch?v=vU0veNETF8s) explanation by Streamlet's creator
