---
description: This describes risks you should be aware of before staking your SYM
---

# Staking Risks

## Misbehaving Staker Nodes

Our blockchain's consensus model relies on staker nodes to propose new blocks. If a staker node was malicious and was allowed to submit any block they wanted, it would be trivially easy to attack the network!&#x20;

To mitigate this type of behavior, the network will **slash** the staked `SYM` on the misbehaving node.&#x20;

{% hint style="danger" %}
If the node you staked SYM on turned out to be malicious, your funds are at risk of being slashed!
{% endhint %}

### How can I tell if a staker node is trustworthy?

Unfortunately, there's no **guaranteed** way to do so, just like there's no surefire way to tell if a centralized exchange or bank will behave honestly.

However, [Melscan](https://scan.themelio.org/) has a UI that shows the total amount of `SYM` that's currently locked up on a particular node. The `number of submitted blocks` and `total value locked` should give you a decent signal of whether the node operator is trustworthy.

### What should I do if my staked funds are slashed?

When a staker node's funds are slashed, there is no way to recover them. Your best option is to stake on another node that has a higher amount of value locked up and/or more blocks successfully submitted.
