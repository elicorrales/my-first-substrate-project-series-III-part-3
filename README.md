# my-first-substrate-project-series-III-part-3

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
  
  
