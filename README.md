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
Binaryen is a compiler and toolchain infrastructure library for WebAssembly, written in C++. It aims to make compiling to WebAssembly easy, fast, and effective.  
  
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
  

Looks like the ```cargo-contract``` (crate?) is the command-line (cli) I was looking for... that help us deploy our smart contract to the node?  
Yup, from  https://crates.io/crates/cargo-contract :  
  
>  
> _cargo-contract  
> Setup and deployment tool for developing Wasm based smart contracts via ink!_  
>  
```
cargo install cargo-contract --force
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
cargo +nightly contract build
```
  

```
npm install -g @polkadot/api-cli #A cli tool to allow you to make API calls to any running node
npm install -g @polkadot/json-serve #A server that serves JSON outputs for specific queries
npm install -g @polkadot/monitor-rpc #A simple monitoring interface that checks the health of a remote node via RPC
npm install -g @polkadot/signer-cli #A cli tool that allows you to generate transactions in one terminal and sign them in another terminal (or computer)
npm install -g @polkadot/vanitygen #Generate vanity addresses, matching some pattern
```

