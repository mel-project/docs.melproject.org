# Use

Now that we've finished writing our `gibbername` crate, we'll demonstrate actually using it in a project. We will build `gibbername-cli`, a trivial wrapper around the library that lets you look up and register names on the command line. Using it willl look something like

```shell-session
gibbername-cli lookup tofnal-seh
hello world my dudes this is what's bound to the name lol
```

You can find a complete example in our [GitHub repo](https://github.com/mel-project/gibbername-cli).

## Project setup

Let's start by creating a new binary crate:

```shell
cargo new gibbername-cli
cd gibbername-cli
```

We add `melprot`, `melstructs`, `anyhow` for error handling,`argh` for lightweight argument parsing, and `futures-lite` for bare-bones async support:

```toml
cargo add melprot melstructs anyhow argh futures-lite
```

We also need to add a dependency on Gibbername itself. This will be a "path" dependency to wherever, locally, you put the Gibbername crate:

```toml
[dependencies]
anyhow = "1.0.69"
argh = "0.1.10"
futures-lite = "1.12.0"
gibbername = { path = "../gibbername" }
melprot = "0.13.3"
melstructs = "0.3.2"
```

## Writing the main function

We write a basic scaffold that parses the arguments with `argh`:

```rust
se argh::FromArgs;
use futures_lite::future::block_on;
use melstructs::{Address, NetID};

#[derive(FromArgs, PartialEq, Debug)]
/// Look up a name in the Gibbername registry.
struct Cli {
    #[argh(option, description = "either 'mainnet' or 'testnet'")]
    network: NetID,
    #[argh(subcommand)]
    command: Command,
}

#[derive(FromArgs, PartialEq, Debug)]
#[argh(subcommand)]
enum Command {
    Lookup(Lookup),
    Register(Register),
}

#[derive(FromArgs, PartialEq, Debug)]
#[argh(subcommand, name = "lookup")]
/// Lookup what is bound to a name
struct Lookup {
    #[argh(positional)]
    name: String,
}

#[derive(FromArgs, PartialEq, Debug)]
#[argh(subcommand, name = "register")]
/// Register a name
struct Register {
    #[argh(option, description = "mel address of the gibbername owner")]
    owner: Address,

    #[argh(option, description = "data to be bound to the gibbername")]
    binding: String,

    #[argh(option, description = "path to the wallet sending the transaction")]
    wallet_path: String,
}

fn main() -> anyhow::Result<()> {
    env_logger::init();
    let args: Cli = argh::from_env();
    // keep around a client
    let client = block_on(melprot::Client::autoconnect(args.network))?;
    match args.command.as_ref() {
        Command::Lookup(lookup) => {
            todo!()
        }
        Command::Register(register) => {
            todo!()
        }
    };
    Ok(())
}
```

## Filling in the functionality

Now that we have a basic scaffold, filling in the functionality is incredibly easy:

```rust
use futures_lite::future::block_on;

fn main() -> anyhow::Result<()> {
    let args: Cli = argh::from_env();
    // keep around a client
    let client = block_on(
        melprot::Client::autoconnect(NetID::Testnet)
    )?;

    match args.command {
        Command::Lookup(lookup) => {
            // we don't need a futures runtime, block_on is fine
            let gname = block_on(gibbername::lookup(&client, &lookup.name))?;
            println!("{gname}");
        }
        Command::Register(register) => {
            // gibbername will prompt the user
            let name = block_on(gibbername::register(&client, register.owner, &register.binding, &register.wallet_name))?;
            println!("registered {:?}", name);
        }
    };
    Ok(())
}
```

## Testing
We now have a complete program! We can test run it with `cargo run`:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>cargo run -- --network mainnet register --owner x7v9tegt6b99xv9t6e56kabap3ych5htw83wa69z0shwa7ms3xbkn7 --binding "hello world" --wallet-path ./alice.json
</strong>Send this command with your wallet: melwallet-cli --wallet-path ./alice.json send --to x7v9tegt6b99xv9t6e56kabap3ych5htw83wa69z0shwa7ms3xbkn7,0.000001,"(NEWCUSTOM)","68656c6c6f20776f726c64" --hex-data 6769626265726e616d652d7631
</code></pre>

Now, run the `melwallet-cli` command which will register our gibbername:

<pre><code><strong>melwallet-cli --wallet-path ./alice.json send --to x7v9tegt6b99xv9t6e56kabap3ych5htw83wa69z0shwa7ms3xbkn7,0.000001,"(NEWCUSTOM)","68656c6c6f20776f726c64" --hex-data 6769626265726e616d652d7631
</strong>registered "segnet-tes"
</code></pre>

Finally, we can look up the binding we set in our `register` command using `lookup`:

<pre><code><strong>cargo run -- --network mainnet lookup segnet-tes
</strong>hello world
</code></pre>
