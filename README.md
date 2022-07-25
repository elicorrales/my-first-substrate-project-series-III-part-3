# my-first-substrate-project-series-III-part-3  
  
Reference: https://docs.substrate.io/tutorials/smart-contracts/first-smart-contract/  
  

```
rustup update
```
  
not convinced yet on this, will hold off doing:
```
rustup component add rust-src --toolchain nightly
```
  
did this already for previous video:
```
rustup target add wasm32-unknown-unknown --toolchain nightly
```

What is ```binaryen``` ?  
> _Binaryen is a compiler and toolchain infrastructure library for WebAssembly, written in C++. It aims to make compiling to WebAssembly easy, fast, and effective._  
  
```
sudo apt install binaryen
```
  
It is beings suggested to install dylint-link:  
  
>
> _dylint-link  
> A wrapper around Rust's default linker to help create Dyling libraries_  
>  
> _cargo-dylint  
> A tool for running Rust lints from dynamic libraries_  
>  
  
The Substrate docs are only suggesting to install the ```dylint-link```.  
It seems the ```cargo-dyint``` is a command-line tool (that uses ```dylint-link```?).  
  
>_A tool for running Rust lints from dynamic libraries_
>
>_```cargo install cargo-dylint dylint-link```_
>
>_Dylint is a Rust linting tool, similar to Clippy. But whereas Clippy runs a predetermined, static set of lints, Dylint runs lints from user-specified, dynamic >libraries. Thus, Dylint allows developers to maintain their own personal lint collection_  
  

```
cargo install dylint-link
```
  
```dylint-link``` is required in order to install ```cargo-contract```.  And ```cargo-contract``` is required if you want to deploy WASM binaries.  
  
>  
> _cargo-contract  
> Setup and deployment tool for developing Wasm based smart contracts via ink!_  
    
```
cargo install cargo-contract
```
  
Once ```cargo-contract``` is installed, you can verify by:  
```
cargo contract --help
```
  
```
cargo contract new flipper
```
  
```
cd flipper
```
  
```
cargo +nightly test
```
  
```
cargo --help | grep list
```
  
OUTPUT:
```
$ cargo --help|grep list
        --list                  List installed commands
Some common cargo commands are (see all commands with --list):
    update      Update dependencies listed in Cargo.lock
```
  
```
cargo --list | grep build
```
  
```
$ cargo build --help|grep target
        --test [<NAME>]             Build only the specified test target
        --bench [<NAME>]            Build only the specified bench target
        --all-targets               Build all targets
        --target <TRIPLE>           Build for the target triple
        --target-dir <DIRECTORY>    Directory for all generated artifacts
```
  

```
cargo build --target wasm32-unknown-unknown --release
```
  

```
cargo +nightly contract build
```
  

```
npm install -g @polkadot/api-cli #A cli tool to allow you to make API calls to any running node
npm install -g @polkadot/json-serve #A server that serves JSON outputs for specific queries
npm install -g @polkadot/monitor-rpc #A simple monitoring interface that checks the health of a remote node via RPC
npm install -g @polkadot/signer-cli #A cli tool that allows you to generate transactions in one terminal and sign them in another terminal (or computer)
npm install -g @polkadot/vanitygen #Generate vanity addresses, matching some pattern
```

