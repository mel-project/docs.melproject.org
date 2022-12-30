---
description: A simple tutorial on how to set up and interact with Themelio wallets
---

# Getting Started

This is a basic guide to `melwallet-cli`, Themelio's reference CLI wallet. We will be funding two testnet wallets, and sending money from one to another.

## Setup and Installation

### Prerequisites

- You’re running a Unix (Linux or macOS) system. The code should work on Windows, but it isn’t as well-tested.
- You have a working Internet connection
- You have `git` installed
- You have the latest Rust toolkit, including the `cargo` command

### Install melwalletd and melwallet-cli

```shell-session
cargo install --locked melwalletd melwallet-cli
```

{% hint style="info" %}
We install not just `melwallet-cli`, but also the `melwalletd` package. This is because `melwallet-cli` is merely a convenient frontend for `melwalletd`, a local daemon that exposes a JSON-RPC interface for managing wallets.
{% endhint %}

### Start melwalletd and connect to a network <a href="#start-melwalletd" id="start-melwalletd"></a>

In a separate terminal (or tmux buffer), start the headless wallet daemon, and connect to the testnet network:

```shell-session
melwallet-cli start-daemon --network testnet
```

`melwalletd` persists and manages wallets and exposes a set of REST endpoints for various wallet operations. You can find more details [here](https://github.com/themeliolabs/melwalletd).&#x20;

This will save any wallets you create to `~/.themelio-wallets`, but feel free to use any path you want.

### Create wallets for Alice and Bob

In your original terminal, create two wallets with `melwallet-cli`:

```shell
melwallet-cli create -w alice
# keep Bob's wallet address handy for the next step!
melwallet-cli create -w bob
```

You will be prompted for passwords, which will be used to encrypt the wallets' private keys at `~/.themelio-wallets/.secrets.json`, so make sure you pick something strong!&#x20;

You can then list the wallets you created like so

```shell-session
melwallet-cli list
```

### Fund Alice's wallet <a href="#fund-wallet" id="fund-wallet"></a>

We can use `faucet` transactions to fund these new testnet wallets. This lets us print MEL out of thin air (and is only available on testnet) for easy testing!&#x20;

```shell-session
melwallet-cli send-faucet -w alice --currency MEL --amount 10000
```

```shell-session
Transaction hash:  9974a514351a0696b6d7e3851da957ff508e44857b4967e3d46b8d16685b9769
(wait for confirmation with melwallet-cli wait-confirmation -w alice 9974a514351a0696b6d7e3851da957ff508e44857b4967e3d46b8d16685b9769)
```

### Send some money to Bob <a href="#send-funds" id="send-funds"></a>

Now, we can transfer some MEL from Alice to Bob!

First we unlock Alice's wallet with her password:

```shell
melwallet-cli unlock -w alice
```

Then, we send over some MEL to Bob's wallet address:

```shell
melwallet-cli send -w alice --to <BOB_ADDRESS>, 500.0
```

Congrats, you've just sent MEL from Alice to bob!

### Further Reading

Sending a transaction is just one of the many things you can do with `melwallet-cli`. For more information, check out our [GitHub](https://github.com/themeliolabs/melwallet-client).
