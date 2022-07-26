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
  
As part of a typical rust/cargo environment, you might have some build abilities for WASM, but it the ```cargo-contract``` seems to be a one-stop shopping.  
  
```cargo-dylint``` is also required when you go to run ```cargo contract build```(further down).  
  
```
cargo install cargo-dylint
```
  
### Possible errors attempting to install cargo-dylint  
  
#### OUTPUT:
```
error: failed to run custom build command for `openssl-sys v0.9.75`
...
...
  run pkg_config fail: "Could not run `\"pkg-config\" \"--libs\" \"--cflags\" \"openssl\"`\nThe pkg-config command could not be found.
  
  Most likely, you need to install a pkg-config package for your OS.
  
  Try `apt install pkg-config`, or `yum install pkg-config`,
  or `pkg install pkg-config` depending on your distribution.
  
  If you've already installed it, ensure the pkg-config command is one of the
  directories in the PATH environment variable.
  
  If you did not expect this build to link to a pre-installed system library,
  then check documentation of the openssl-sys crate for an option to
  build the library from source, or disable features or dependencies
  that require pkg-config."
```

#### Solution:  
```
 sudo apt install -y pkg-config
 ```

#### OUTPUT:  
```
 error: failed to run custom build command for `openssl-sys v0.9.75`
...
...
run pkg_config fail: "`\"pkg-config\" \"--libs\" \"--cflags\" \"openssl\"` did not exit successfully: exit status: 1
error: could not find system library 'openssl' required by the 'openssl-sys' crate
--- stderr
Package openssl was not found in the pkg-config search path.
Perhaps you should add the directory containing `openssl.pc'
to the PKG_CONFIG_PATH environment variable
No package 'openssl' found"
```
  
#### Solution:  
```
sudo apt install -y libssl-dev
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
  
So the above gives us ```--list```.  
  

```
cargo --list | grep build
```
  
OUTPUT:
```
$ cargo --list | grep build
    b                    alias: build
    build                Compile a local package and all of its dependencies
    build-bpf
    test                 Execute all unit and integration tests and build examples of a local package
```
  
Notice there is no reference (above) to anything WASM.  
  
```
cargo build --help|grep target
```
  
OUTPUT:  
```
$ cargo build --help|grep target
        --test [<NAME>]             Build only the specified test target
        --bench [<NAME>]            Build only the specified bench target
        --all-targets               Build all targets
        --target <TRIPLE>           Build for the target triple
        --target-dir <DIRECTORY>    Directory for all generated artifacts
```
  
The ```--target <TRIPLE>``` is what can build a WASM binary.  
  
Example:  
```
cargo build --target wasm32-unknown-unknown --release
```
  
However, we have no way to deploy it to any blockchain.  
  
Thus, ```cargo-contract```.  
  

```
cargo install cargo-contract
```
  
Once ```cargo-contract``` is installed, you can verify by:  
```
cargo contract --help
```
  
```
$ cargo contract --help
cargo-contract 1.4.0-unknown-x86_64-unknown-linux-gnu
Utilities to develop Wasm smart contracts

USAGE:
    cargo contract <SUBCOMMAND>

SUBCOMMANDS:
    new            Setup and create a new smart contract project
    build          Compiles the contract, generates metadata, bundles both together in a
                       `<name>.contract` file
    upload         Upload contract code
    instantiate    Instantiate a contract
    call           Call a contract
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
This (below) may seem to work but issues further down.  
```
cargo build --target wasm32-unknown-unknown --release
```
  
```
cargo contract upload
```
  
OUTPUT:
```
error: The following required arguments were not provided:
    --suri <suri>
```
  

```
cargo contract upload --suri asdf
```
  
OUTPUT:
```
ERROR: No 'ink_lang' dependency found
```
  
Edit Cargo.toml:  Enable the ```[dependencies]``` section and make sure it has:  
```
[dependencies]
#cargo contract uplodad (deployment) will not work without this:
ink_lang = { version = "3", default-features = false }
```
  
Try again:  
```
cargo contract upload --suri asdf
```
  
OUTPUT:  
```
ERROR: Metadata file not found. Try building with `cargo contract build`.
```
  
  
```
cargo +nightly contract build # <---???is nightly necessary?
cargo contract build
```
  
YES! ```cargo contract build``` alone causes error: 
```
ERROR: cargo-contract cannot build using the "stable" channel. Switch to nightly.
See https://github.com/paritytech/cargo-contract#build-requires-the-nightly-toolchain
```
  
OUTPUT:  
```
$ cargo +nightly contract build
 [1/5] Checking ink! linting rules
Checking with toolchain `nightly-2022-03-14-x86_64-unknown-linux-gnu`
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
 [2/5] Building cargo project
    Updating crates.io index
error: "/home/IamDeveloper/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/Cargo.lock" does not exist, unable to build with the standard library, try:
        rustup component add rust-src --toolchain nightly-x86_64-unknown-linux-gnu
ERROR: `"/home/IamDeveloper/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/bin/cargo" "build" "--target=wasm32-unknown-unknown" "-Zbuild-std" "--no-default-features" "--release" "--target-dir=/home/IamDeveloper/MySoftwareProjects/blockchain/rust/rust-substrate-blockchain-projects/my-first-substrate-projects/my-first-project-prep-lesson/my-first-smart-contracts/test/target/ink" "--features=ink_env/ink-debug"` failed with exit code: Some(101)
```
  
So we check what we have with ```rustup show```.  
OUTPUT:
```
$ rustup show
Default host: x86_64-unknown-linux-gnu
rustup home:  /home/IamDeveloper/.rustup

installed toolchains
--------------------

stable-x86_64-unknown-linux-gnu (default)
nightly-2022-03-14-x86_64-unknown-linux-gnu
nightly-x86_64-unknown-linux-gnu
bpf

installed targets for active toolchain
--------------------------------------

wasm32-unknown-unknown
x86_64-unknown-linux-gnu

active toolchain
----------------

stable-x86_64-unknown-linux-gnu (default)
rustc 1.62.1 (e092d0b6b 2022-07-16)
```
  
Thus we need to take above suggestion:
```
rustup component add rust-src --toolchain nightly-x86_64-unknown-linux-gnu
```
  
After the above, we re-run:
```
cargo +nightly contract build
```
  
OUTPUT: (the "Compiling test" refers to our temp smart contract project that we created):
```
   Compiling test v0.1.0 (/tmp/cargo-contract_pDkwu4)
    Finished release [optimized] target(s) in 51.75s
 [3/5] Post processing wasm file
 [4/5] Optimizing wasm file
ERROR: wasm-opt not found! Make sure the binary is in your PATH environment.

We use this tool to optimize the size of your contract's Wasm binary.

wasm-opt is part of the binaryen package. You can find detailed
installation instructions on https://github.com/WebAssembly/binaryen#tools.

There are ready-to-install packages for many platforms:
* Debian/Ubuntu: apt-get install binaryen
* Homebrew: brew install binaryen
* Arch Linux: pacman -S binaryen
* Windows: binary releases at https://github.com/WebAssembly/binaryen/releases
IamDeveloper@SoftwareDevelopUbuntu2004
$
```
  
Let's do a cargo crate search:
```
$ cargo search wasm-opt
wasm-opt = "0.0.0"        # wasm-opt bindings
wasm-opt-sys = "0.0.0"    # Native wasm-opt build
xtask-wasm = "0.1.5"      # Customizable subcomman
```
  
Looks like the 1st one in the list.  
```
cargo install wasm-opt
```
  
Try ```cargo +nightly contract build``` again.  
  
OUTPUT:
```
$ cargo +nightly contract build
 [1/5] Checking ink! linting rules
Checking with toolchain `nightly-2022-03-14-x86_64-unknown-linux-gnu`
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
 [2/5] Building cargo project
    Updating crates.io index
    Finished release [optimized] target(s) in 0.25s
 [3/5] Post processing wasm file
 [4/5] Optimizing wasm file
ERROR: Unable to extract version information from ''.
Your wasm-opt version is most probably too old. Make sure you use a version >= 99.

If you tried installing from your system package manager the best
way forward is to download a recent binary release directly:

https://github.com/WebAssembly/binaryen/releases

Make sure that the `wasm-opt` file from that release is in your `PATH`.
```
  
Go to https://github.com/WebAssembly/binaryen/releases, find the latest release,  
in this example, it is ```binaryen-version_109-x86_64-linux.tar.gz```.
```
  
```
binaryen-version_109-x86_64-linux.tar.gz
$ mkdir temp
$ mv binaryen-version_109-x86_64-linux.tar.gz temp/
$ cd temp/
$ tar -xvzf binaryen-version_109-x86_64-linux.tar.gz
$ cd binaryen-version_109/
$ ls -l
total 12
drwxr-xr-x 2 IamDeveloper IamDeveloper 4096 Jun 14 14:14 bin
drwxr-xr-x 2 IamDeveloper IamDeveloper 4096 Jun 14 14:11 include
drwxr-xr-x 2 IamDeveloper IamDeveloper 4096 Jun 14 14:11 lib
$ sudo cp -r bin/* /bin
$ sudo cp -r include/* /usr/include/
$ sudo cp -r lib/* /lib64
```
    



OUTPUT:
```
ERROR: Mismatching versions of `scale-info` were found!
Please ensure that your contract and your ink! dependencies use a compatible version of this package.
```
  


git clone https://github.com/paritytech/substrate.git
```
  
```
cd substrate
```
  
```
cargo +nightly build --package subkey --release #??? is this necessary?
cargo build --package subkey --release
```
  
```
 ./target/release/subkey generate
 ```
   
 OUTPUT:  
 ```
 Secret phrase:       sell holiday brief music walk screen twist maple load crucial busy bottom
  Network ID:        substrate
  Secret seed:       0x439231d9f13b2e57603312db7dc3704ae7c1d6a353c9bf5d982051f69d870811
  Public key (hex):  0xae344b6e58b5d68105f4d8bf986128f827e034e42644a3c8f6067ffc2ee96374
  Account ID:        0xae344b6e58b5d68105f4d8bf986128f827e034e42644a3c8f6067ffc2ee96374
  Public key (SS58): 5G17iwJaXvBWx8evNGX929WyGzXLaRt9x2mJU8WEPLg9Yd7t
  SS58 Address:      5G17iwJaXvBWx8evNGX929WyGzXLaRt9x2mJU8WEPLg9Yd7t
  ```
    
  
```
npm install -g @polkadot/api-cli #A cli tool to allow you to make API calls to any running node
npm install -g @polkadot/json-serve #A server that serves JSON outputs for specific queries
npm install -g @polkadot/monitor-rpc #A simple monitoring interface that checks the health of a remote node via RPC
npm install -g @polkadot/signer-cli #A cli tool that allows you to generate transactions in one terminal and sign them in another terminal (or computer)
npm install -g @polkadot/vanitygen #Generate vanity addresses, matching some pattern
```

