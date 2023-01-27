---
description: Here, we'll show you how to create a Rust library with melprot as a dependency
---

# Project setup

## Create your Rust library

### Prerequisites

Make sure you have Rust and `cargo` installed. If not, follow the official instructions [here](https://www.rust-lang.org/tools/install). If you'd like, also install `cargo-edit` for a simple way to configure dependencies from the command line.

```shell-session
$ cargo install cargo-edit --features vendored-openssl
```

{% hint style="info" %}
`cargo-edit` gives you some useful sub-commands like `cargo add <crate>` and `cargo rm <crate>`
{% endhint %}

### Create your your library with cargo

```shell-session
$ cargo new --lib <project-name>
```

{% hint style="info" %}
The `--lib` flag will tell `cargo` to create a library instead of a binary (by default).
{% endhint %}

This should create a barebons directory with a few starter files including `Cargo.toml` and `lib.rs`.

### Adding melprot as a dependency

In `Cargo.toml`, we can specify external dependencies for our library under the dependencies section. You can edit the file manually, or use `cargo add melprot`

```toml
[dependencies]
melprot = <version>
```
