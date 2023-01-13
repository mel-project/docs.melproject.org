---
description: >-
  Using a faucet transaction unique to our testnet, you can print money out of
  thin air to fund your testnet wallet.
---

# 💰 Fund your wallet

## Prerequisites

* You’re running a Unix (Linux or macOS) system (the code should work on Windows, but it isn’t as well-tested)
* You have a working Internet connection
* You have Git installed
* You have the latest Rust toolkit, including the `cargo` command

### Install melwalletd and melwallet-cli

```shell-session
cargo install --locked melwalletd melwallet-cli
```

{% hint style="info" %}
We install both packages because `melwallet-cli` is merely a convenient frontend _for_ `melwalletd`, a local daemon that exposes a JSON-RPC interface for managing wallets. You can find more details [here](https://github.com/themeliolabs/melwalletd).
{% endhint %}

### Start melwallet-cli <a href="#start-melwalletd" id="start-melwalletd"></a>

In a separate terminal, start the headless wallet daemon and connect to the testnet network:

```shell-session
melwallet-cli start-daemon --network testnet
```

This command will save any wallets you create to `~/.themelio-wallets`, but feel free to provide any directory you want via the `--wallet-dir` flag.

## Funding your wallet

You can use the `send-faucet` command to get some funds into your wallet, specifying the amount and currency type.

{% hint style="info" %}
Every transaction costs a small amount of `MEL` to send because of the transaction fee, so always make sure you have some `MEL` your wallet before attempting a transaction.
{% endhint %}

For example, to print `MEL`:

```shell-session
$ melwallet-cli send-faucet -w <wallet-name> --currency MEL --amount 10000
```

To print `SYM`:

```shell-session
$ melwallet-cli send-faucet -w <wallet-name> --currency SYM --amount 10000
```

That's it! Now you have some testnet money to play with :tada:
