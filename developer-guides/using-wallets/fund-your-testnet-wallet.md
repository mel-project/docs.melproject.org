---
description: >-
  Using a faucet transaction unique to our testnet, you can print money out of
  thin air to fund your testnet wallet.
---

# Fund your testnet wallet

## Start melwallet-cli <a href="#start-melwalletd" id="start-melwalletd"></a>

In a separate terminal, start the headless wallet daemon and connect to the testnet network:

```shell-session
$ melwallet-cli start-daemon --network testnet
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
