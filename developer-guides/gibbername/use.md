# Use

Now that we've finished writing our `gibbername` crate, we'll demonstrate actually using it in a project. We will build `gibbername-cli`, a trivial wrapper around the library that lets you lookup and register names on the command line. Using it willl look something like

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

We add `melprot`, `argh` for lightweight argument parsing, and `futures-lite` for bare-bones async support:

```toml
$ cargo add melprot, argh futures-util
    Updating crates.io index
      Adding melprot v0.1.0 to dependencies.
      Adding argh v0.1.10 to dependencies.
      Adding futures-lite 1.12.0 to dependencies.
```

We also need to add a dependency on Gibbername itself. This will be a "path" dependency to wherever, locally, you put the Gibbername crate:

```toml
[dependencies]
argh = "0.1.10"
futures-lite = "1.12.0"
gibbername = { path = "../some/path/to/gibbername" }
```

## Writing the main function

We write a basic scaffold that parses the arguments with `argh`:

```rust
use argh::FromArgs;
use melstructs::NetID;

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
    let client = melprot::Client::autoconnect(NetID::Mainnet);
    match args.command.as_ref() {
        Command::Lookup(lookup) => {
            // we don't need a futures runtime, block_on is fine
            let gname = block_on(gibbername::lookup(&client, &lookup.name))?;
            println!("{gname}");
        }
        Command::Register(register) => {
            // gibbername will prompt the user
            let name = block_on(gibbername::register(&client, &register.owner, &register.binding))?;
            println!("registered {:?}", name);
        }
    };
    Ok(())
}
```

## Testing

We now have a complete program! We can test run it with `cargo run`:

```shell-session
$ cargo run -- lookup tofnal-qujjay-seh
hello world my dudes this is what's bound to the name lol
$ cargo run -- register --owner t1m9v0fhkbr7q1sfg59prke1sbpt0gm2qgrb166mp8n8m59962gdm0 --binding foobar
send with your wallet: melwallet:.............................
```
