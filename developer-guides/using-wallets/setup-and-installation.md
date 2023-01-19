---
description: A short guide on how to set up and install our wallet CLI.
---

# Setup and Installation

## Prerequisites

* You’re running a Unix (Linux or macOS) system (the code should work on Windows, but it isn’t as well-tested)
* You have a working internet connection
* You have Git installed
* You have the latest Rust toolkit, including the `cargo` command

## Install melwalletd and melwallet-cli

```shell-session
$ cargo install --locked melwalletd melwallet-cli
```

{% hint style="info" %}
We install both packages because `melwallet-cli` is merely a convenient frontend _for_ `melwalletd`, a local daemon that exposes a JSON-RPC interface for managing wallets. You can find more details [here](https://github.com/themeliolabs/melwalletd).
{% endhint %}
