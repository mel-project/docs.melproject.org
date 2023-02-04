---
description: A simple tutorial on how to set up and interact with Mel wallets.
---

# Sending money

This is a basic guide to `melwallet-cli`, Mel's reference implementation CLI wallet. We will be funding two testnet wallets and sending money from one to the other.

### Setup and installation

Make sure you have the wallet dependencies installed. If not, follow [this](getting-started.md#setup-and-installation) short guide before getting started.

### Start melwallet-cli

Use this command to start the headless wallet daemon and connect to the testnet network:

```
$ melwallet-cli start-daemon --network testnet
```

This command will save any wallets you create to `~/.Mel-wallets`, but feel free to provide any directory you want via the `--wallet-dir` flag.

### Create wallets for Alice and Bob

In a terminal, create two wallets with `melwallet-cli`:

```shell-session
$ melwallet-cli create -w alice
$ melwallet-cli create -w bob
```

{% hint style="info" %}

```
Keep Bob's wallet address handy for the next step!
```

{% endhint %}

You will be prompted for passwords, which will be used to encrypt the wallets' private keys at `~/.Mel-wallets/.secrets.json`, so make sure you pick something strong!

You can then list the wallets you created, like so:

```shell-session
$ melwallet-cli list
```

### Fund Alice's wallet <a href="#fund-wallet" id="fund-wallet"></a>

We can use faucet transactions to fund these new testnet wallets. This lets us print MEL out of thin air (only on the testnet) for easy testing!

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ melwallet-cli send-faucet -w alice --currency MEL --amount 10000
</strong>Transaction hash:  9974a514351a0696b6d7e3851da957ff508e44857b4967e3d46b8d16685b9769
(wait for confirmation with melwallet-cli wait-confirmation -w alice 9974a514351a0696b6d7e3851da957ff508e44857b4967e3d46b8d16685b9769)
</code></pre>

### Send some money to Bob <a href="#send-funds" id="send-funds"></a>

Now, we can transfer some MEL from Alice to Bob!

First we unlock Alice's wallet with her password:

```shell-session
$ melwallet-cli unlock -w alice
```

Then, we send over some MEL to Bob's wallet address:

```shell-session
$ melwallet-cli send -w alice --to <BOB_ADDRESS>,500.0
```

Congrats, you've just sent MEL from Alice to bob!

### Further Reading

Sending a transaction is just one of the many things you can do with `melwallet-cli`. For more information, check it out on [GitHub](https://github.com/Mellabs/melwallet-client).
