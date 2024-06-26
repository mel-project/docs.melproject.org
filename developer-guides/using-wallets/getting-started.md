---
description: A simple tutorial on how to set up and interact with Mel wallets.
---

# Sending money

This is a basic guide to `melwallet-cli`, Mel's reference implementation CLI wallet. We will be funding two testnet wallets and sending money from one to the other.

## Setup and installation

Make sure you have the wallet installed. If not, follow [this](getting-started.md#setup-and-installation) short guide.

## Create wallets for Alice and Bob

In a terminal, create two *testnet* wallets at your selected path:

```shell-session
melwallet-cli --wallet-path ./alice.json create --network testnet
melwallet-cli --wallet-path ./bob.json create --network testnet
```

{% hint style="info" %}

The wallets with their secrets are stored as *unencrypted* json documents on disk. Users and applications should take care to encrypt as needed.

{% endhint %}

## Fund Alice's wallet <a href="#fund-wallet" id="fund-wallet"></a>

Let's use the faucet to print testnet MEL to fund our new testnet wallets. This command sends 1000 MEL to wallet `alice` and waits until the transaction is confirmed:

```shell-session
melwallet-cli --wallet-path ./alice.json send-faucet --wait
```

## Send some money to Bob <a href="#send-funds" id="send-funds"></a>

Now, we transfer some MEL from alice to bob. First, obtain bob's address using

```shell-session
melwallet-cli --wallet-path ./bob.json summary
```

You should get output similar to
```
Network:  testnet
Address:  t7v9tegt6bm6dv9t6e56ktdap3ych5htw83wa69z0shwa7nt3xbkn0
Balances:
```

Send the money to bob:

```shell-session
melwallet-cli --wallet-path ./alice.json send --to <BOB_ADDRESS>,500.0 --wait
```

This command sends `500.0` MEL from alice to bob and waits for the transaction to confirm. When it returns, you have successfully sent MEL to bob!

## Further reading

Sending a transaction is just one of the many things you can do with `melwallet-cli`. For more information, check it out on [GitHub](https://github.com/mel-project/melwallet-client).
