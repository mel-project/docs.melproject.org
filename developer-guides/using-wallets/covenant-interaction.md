---
description: >-
  This page will guide a user through creating, deploying, and interacting with
  a Themelio covenant using a simple example.
---

# Covenant interaction

## About Themelio covenants

Covenants are Themelio’s equivalent of “smart contracts” in its coin-based model. A covenant, somewhat analogous to a Bitcoin script, is a program attached to a coin (UTXO) that takes in a transaction and its execution context and decides whether or not that transaction can spend.

On the blockchain itself, contracts are written in MelVM, a low-level stack-based “machine” language fulfilling a similar role as EVM in Ethereum. Unlike coin scripts in most other UTXO blockchains, MelVM covenants are general-purpose, meaning that they can arbitrarily control the spending of a coin and are not limited to simple spending conditions like multi-signature requirements. This allows for essentially all on-chain constructs to be built on top of Themelio, such as naming systems, DeFi contracts, etc.

## Prerequisites

In the first section we used Alice's wallet to request MEL using the testnet faucet. In this section we are going to use that same wallet to deploy a covenant and later interact with it. To follow along make sure you have at least 100 `MEL`.

## Install melorun

```
cargo install --locked melorun
```

## Creating a covenant

Though Themelio covenants are written in MelVM, Melodeon is a high-level language created specifically to facilitate Themelio covenant creation. It compiles to MelVM but is much easier to work with.

To get started we are going to write a very simple "password" covenant which will allow use to lock up coins on the testnet with a precomputed Blake3 hash of our password.

{% hint style="info" %}
Please note, this covenant is **not** production ready. We do not recommend you use this covenant to lock up any coins on mainnet, you will likely get frontrun and lose your coins.
{% endhint %}

Let's start by first hashing our password using the melorun REPL; use `melorun -i` to start it and hash your password by passing it in to the Blake3 function: `blake3("your password here")` (tip: if you want to write your password using raw hexadecimal, prepend your string with an `x` like this: `x"48656c6c6f20776f726c6421"`). In this example, I'm going to be using the password `Hello world!` which results in the hash `x"793c10bc0b28c378330d39edace7260af9da81d603b8ffede2706a21eda893f4"`.

Next, we create the Melodeon file we're going to use to write our covenant, in this case, we will name it `password.melo`. It can be written with only a few lines of code:

```
def get_data(): %[] =
    unsafe (vref(env_spender_tx().outputs, 0) :! CoinData).additional_data

---

unsafe b2n(blake3(get_data() :! %[12])) == b2n(x"793c10bc0b28c378330d39edace7260af9da81d603b8ffede2706a21eda893f4")
```

Let's look at our covenant in more depth:

* the first section defines a function named `get_data()` which retrieves the byte vector stored in the additional data of the transaction trying to spend the coin
* the three dashes separating the two main parts of our covenant divide the function definition section of our Melodeon program from the covenant entry point (similar to the `main()` function present in many other programming languages)
* the last section hashes the transaction's additional data, and checks that it matches the hash of the password we calculated earlier

{% hint style="info" %}
Because Melodeon programs are bounded, we must specify the length of our password with _`:! $[YOUR_PASSWORD_LENGTH]`_. Make sure you replace the _`12`_ in our example with the length of your password, in bytes.
{% endhint %}

## Testing covenants before deployment

Both melorun and the online playground support specifying the spend context. This is a special, YAML-formatted document that allows you to specify a particular spending transaction without constructing every field of it manually — see its [spec](https://github.com/themeliolabs/melorun).

You can apply this spend environment with the `-s` flag of melorun, or just use the [playground](https://play.melodeonlang.org/#DgAAAODMzMwxzABlZTHMAGVlMQ).

We'll name our spend context file `context.yaml` and it will contain information about a mock transaction. The only value we really care about here is the `additional_data` field, which will contain our password in bytes; this means that instead of putting `Hello world!` we are going to use it's equivalent in hexadecimal: `48656c6c6f20776f726c6421`.

```
spender_outputs:
  0:
    covhash: t6jjw53gj6caa9dhqrefytb7pj53rz8pqfnnjxh5m7x64pfjd4h6n1
    denom: MEL
    value: 1337
    additional_data: 48656c6c6f20776f726c6421
```

We can now test that everything works as expected using this command: `melorun password.melo -s context.yaml`. If it returns&#x20;

```
- : Nat [more specifically: {0..1}]
1 (truthy)
```

then the covenant and password in our YAML file match and we're ready to deploy.

## Deploying on the testnet

The next step now is to test our covenant live on the official [testnet](https://scan-testnet.themelio.org/).

To do that, we need to first compile our Melodeon program to MelVM bytecode. This can be done with the `-`c (or `--compile`) flag of melorun; the full command would be `melorun -c password.melo -s context.yaml`.

You’ll see something like the following two lines on stdout:

```
t7m8hrg0vwex2h59a62qm48xr0g1jryatas74ycrhxep85e65x1cv0
f20103f2010242000050430022f2004300234200224300244200245342002325a1000842002243002542002343002642002642002550a00001f2005043002742002730000c430028420028c1430021f020793c10bc0b28c378330d39edace7260af9da81d603b8ffede2706a21eda893f443002a42002ac143002942002942002124
```

The first line is the hash of the covenant in the standard “address” format, while the second line contains the hex-encoded MelVM bytecode.

## Locking up a coin with our covenant

The next step is to lock up a coin with this covenant.

Let’s start melwalletd in testnet mode if it's not already running with `melwallet-cli start-daemon --network testnet` and use melwallet-cli to send 100 testnet `MEL` to the covenant hash we saw earlier:

```
melwallet-cli send -w alice --to t7m8hrg0vwex2h59a62qm48xr0g1jryatas74ycrhxep85e65x1cv0

# TRANSACTION RECIPIENTS
# Address                                                 Amount          Additional data
# t7m8hrg0vwex2h59a62qm48xr0g1jryatas74ycrhxep85e65x1cv0  100.000000 MEL  ""
# t92zte6gh26y5s02g9h31vgmnv0q4vya8paw5vghfe6c5b4hvj28pg  8.187089 MEL    ""
# t92zte6gh26y5s02g9h31vgmnv0q4vya8paw5vghfe6c5b4hvj28pg  8.187089 MEL    ""
#  (network fees)                                         0.000395 MEL
# Proceed? [y/N]
y

# Transaction hash:  630024d8f526fa15cb53d40aec440e63ae32c636229696e312e66f311fee7c6b
# (wait for confirmation with melwallet-cli wait-confirmation -w demo 630024d8f526fa15cb53d40aec440e63ae32c636229696e312e66f311fee7c6b)
```

After this transaction confirms, the 100 `MEL` will be locked up at the first output of the transaction with hash `630024d8f526fa15cb53d40aec440e63ae32c636229696e312e66f311fee7c6b` . It can only be spent by a transaction that satisfies the covenant we wrote earlier. (You can even see it on Melscan — the above is a transcript of an actual testnet deployment!)

## Spending the locked coin

To test our covenant in action, the next step is to create a transaction that satisfies the covenant.

Note that in the following steps, we use bash/zsh syntax for string interpolation to avoid repeatedly typing long strings. On Windows, Git Bash is a good way of accessing bash syntax.

Using any wallet (Bob's, Alice's, or even Carol's), run the following melwallet-cli command:

```
melwallet-cli send -to t22112s3z287v74fhp4z3kbsjq48y7fxez59tjr0d6xx7p71cxpe5g,100,MEL,asci="Hello world" --force-spend 630024d8f526fa15cb53d40aec440e63ae32c636229696e312e66f311fee7c6b-0 --add-covenant f20103f2010242000050430022f2004300234200224300244200245342002325a1000842002243002542002343002642002642002550a00001f2005043002742002730000c430028420028c1430021f020793c10bc0b28c378330d39edace7260af9da81d603b8ffede2706a21eda893f443002a42002ac143002942002942002124
```

Let’s unpack this command a little. It asks the demo wallet to format a transaction that

* sends 100 `MEL` to `t22112s3z287v74fhp4z3kbsjq48y7fxez59tjr0d6xx7p71cxpe5g`, the address for Bob that was created for this tutorial, here you would put an address that you created
* adds our password to the additional data of our transaction and automatically converts it to hexadecimal because we prepended it with `ascii=`
* spends the covenant-locked coin we just created earlier — the out-of-wallet coin `630024d8f526fa15cb53d40aec440e63ae32c636229696e312e66f311fee7c6b-0`
* supplies the content of the covenant inline as a large hex string

{% hint style="info" %}
Note: We must provide the covenant bytecode in our transaction because when we created the coin, we locked it with the covenant through its covenant hash. When we spend the coin, we then need to supply the actual contents of the covenant. This construct, borrowed from Bitcoin’s “pay-to-script-hash”, ensures that bulky covenants do not burden the coin state (and instead only need to be archived in the blockchain history, which does not need frequent access).
{% endhint %}

## Deploying on the mainnet

Following the same steps with a mainnet melwalletd and mainnet money, you can deploy the same covenant which locks “real money” as well, although we recommend you don't deploy this particular covenant to mainnet.

You can see a more in-depth example of covenant for a multi-signature wallet which is ready for production [here](https://guide.melodeonlang.org/9\_deploying\_covenants.html).
