# Melnode quick start

**Full nodes** replicate every consensus-confirmed block, validating their contents and ensuring network security while providing a local cache of the entire blockchain state. Running a full node helps contribute to the security and performance of the network.

**Melnode** is the reference implementation of a Themelio full node. This guide will help you set up the environment needed to run Melnode.

## Hardware requirements

### Minimum

* 1-core CPU
* 4 GB of RAM
* at least 200 GB of free storage (SSD not necessary)
* 10 Mbps download Internet service

### Recommended

* 4+ core CPU
* 16 GB of RAM
* 200+ GB of free storage on a fast device (SSD, RAID array, etc)
* 50+ Mbps up/download Internet service

## Install Rust and Cargo

For security reasons, until we have reliable reproducible build infrastructure, we stick to releasing source code and do not distribute any official binary packages.

Fortunately, Rust's package manager Cargo is _very_ easy to use, and most likely easier to use than whatever package manager you already use.

Follow the [instructions](https://doc.rust-lang.org/cargo/getting-started/installation.html) from the official Cargo Book to get started. Make sure that the `cargo` command is available and of the latest version:

<pre class="language-shell-session"><code class="lang-shell-session"><strong>$ cargo version
</strong>cargo 1.65.0 (4bc8f24d3 2022-10-20)
</code></pre>

## Compile and install melnode

Simply run the following command:

```shell-session
$ cargo install --locked melnode
```

{% hint style="info" %}
Don't forget the `--locked` parameter! That ensures that all dependencies are locked to the specific version we specify, which can sometimes be important for correct functionality.
{% endhint %}

This should kick off a fairly long build process, but eventually you should see something like this, indicating the `melnode` has been installed successfully:

```shell-session
   Compiling themelio-bootstrap v0.6.1
   Compiling imbl v1.0.1
   Compiling lz4_flex v0.8.2
   Compiling arc-swap v1.5.1
   Compiling clone-macro v0.1.0
   Compiling jemallocator v0.3.2
   Compiling jemallocator-global v0.3.2
   Compiling rusqlite v0.26.3
   Compiling boringdb v0.4.1
   Compiling melnode v0.14.0
    Finished release [optimized] target(s) in 1m 29
```
