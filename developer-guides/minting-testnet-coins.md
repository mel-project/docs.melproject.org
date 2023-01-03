# Fund Your Wallet
This is a short guide on how to fund your testnet wallet. Using a faucet transaction, unique to our testnet, you can print money out of thin air. 

## Prerequisites

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


## Printing SYM 
For example, to print SYM:
```shell-session
$ melwallet-cli send-faucet -w <wallet-name> --currency SYM --amount 10000
```

That's it! Now you have some testnet money to play with. To learn more about swapping, take a look at our quick [Melswap guide](../developer-guides/using-wallets/melswap-guide.md).
