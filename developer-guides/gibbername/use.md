# Use

Now that we've finished writing our `gibbername` crate, we'll demonstrate actually using it in a project. We will build `gibbername-cli`, a trivial wrapper around the library that lets you look up and register names on the command line. Using it willl look something like

```shell-session
$ gibbername-cli lookup tofnal-qujjay-seh
hello world my dudes this is what's bound to the name lol
```

## Project setup

Let's start by creating a new binary crate:

```shell
$ cargo new gibbername-cli
$ cd gibbername-cli
```

We add `melprot`, `melstructs`, `anyhow` for error handling,`argh` for lightweight argument parsing, and `futures-lite` for bare-bones async support:

```toml
$ cargo add melprot melstructs argh futures-lite
    Updating crates.io index
      Adding melprot v0.1.0 to dependencies.
      Adding melstructs v0.3.2 to dependencies.
      Adding anyhow v1.0.69 to dependencies.
      Adding argh v0.1.10 to dependencies.
      Adding futures-lite 1.12.0 to dependencies.
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
use argh::FromArgs;
use melstructs::{Address, NetID};

#[derive(FromArgs, PartialEq, Debug)]
/// Look up a name in the Gibbername registry.
struct Cli {
    #[argh(subcommand)]
    command: Command,
}

#[argh(subcommand)]
enum Command {
    Lookup(Lookup),
    Register(Register)
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
    #[argh(option)]
    owner: Address,

    #[argh(option)]
    binding: String
}


fn main() -> anyhow::Result<()> {
    let args: Cli = argh::from_env();
    // keep around a client
    let client = melprot::Client::autoconnect(NetID::Mainnet);
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
    let client = futures_lite::future::block_on(
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
            let name = block_on(gibbername::register(&client, register.owner, &register.binding))?;
            println!("registered {:?}", name);
        }
    };
    Ok(())
}
```

## Testing

We now have a complete program! We can test run it with `cargo run`:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ cargo run -- register --owner t1cj51xmq3dxn91z8exz3vhbk2wc8g9enh3kzsbmd3zzy6yx1memyg --binding 'hello CLI'
</strong>Send this command with your wallet: melwallet-cli send -w last --to t1cj51xmq3dxn91z8exz3vhbk2wc8g9enh3kzsbmd3zzy6yx1memyg,0.000001,"(NEWCUSTOM)","68656c6c6f20434c49" --hex-data 6769626265726e616d652d7631
</code></pre>

Now we run the `melwallet-cli` command which will register our gibbername:

<pre><code><strong>$ melwallet-cli send -w last --to t1cj51xmq3dxn91z8exz3vhbk2wc8g9enh3kzsbmd3zzy6yx1memyg,0.000001,"(NEWCUSTOM)","68656c6c6f20434c49" --hex-data 6769626265726e616d652d7631
</strong>registered "zewses"
</code></pre>

Finally, we can look up the binding we set in our `register` command using `lookup`:

```
$ cargo run -- lookup zewses
hello CLI
```
