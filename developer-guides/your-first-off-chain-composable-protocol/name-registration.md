# Name registration

Make a transaction where the first two outputs are:

* `NewDenom`, 1, arbitrary covenant
* `NewDenom`, 1, fixed unspendable covenant (`0xaaaaaaaa..`)

Rust code for creating a wallet URL. Library function, plus a simple example program.

\----

Using our \<TODO>, we can create a wallet URL:

```rust
let wallet_url = client.get_wallet_url().await?;
// TODO: show some example of how to use the wallet URL
```

Now, we can send a transaction with the first two outputs as:

1. TODO, come up with a name for the denom
2. same as above - but make sure this has an unspendable covenant!

Why do we need two separate outputs?

TODO: explanation
