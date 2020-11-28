## Learning Rust
* Unlike other languages, it is very tricky to learn Rust by directly jump into coding.
* This is the first con about the Rust language.
* We need to understand few important concepts of Rust, before we start writing some code samples
* Otherwise understanding the code snippets from code samples would quickly become frustating and boring. 

## Learning Resourses
Few important web links and books to start with:
* **The Rust Programming Language**
* **Rust By Example**
* **Rustlings**: The command line version of Rust Guide
* It is a good idea to finish the fitst 4 chapters of **The Rust Programming Language** completely so that we can get familiar with most imoprtant rust language concepts

## Tools required
* **rustup** : Tool to upgrade to the latest version of the rust tool chain

* **cargo**  : A complete tool to do most tasks like build, package management etc. during development

## How to install Rust tool chain?
All you need to do is run the following command in any Nix OS; This will install tools like rust compiler and package manager

> ```curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh```

* Check the rust compiler

```
$ rustc
Usage: rustc [OPTIONS] INPUT

Options:
    -h, --help          Display this message

...

```


* Check Package manager **Cargo**

```
$ cargo
Rust's package manager

USAGE:
    cargo [+toolchain] [OPTIONS] [SUBCOMMAND]

OPTIONS
...
```

## How to update the Rust tool chain

```
$ rustup update
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
info: checking for self-updates

  stable-x86_64-unknown-linux-gnu unchanged - rustc 1.47.0 (18bf6b4f0 2020-10-07)

info: cleaning up downloads & tmp directories
```

## Rust Tool chain is ready for development
* `rustc` is a compiler
* `cargo` is a package manager
* We generally dont use the compiler `ructc` directly but we use `cargo` for most of the development; 
* Go to next section for more details about the `cargo`

## The Cargo Tool
#### Create projects
> ```
> cargo new project_hello
> cd project_hello/
> ```

#### Check the code for error and warning without build
> `cargo check`
> #### Build the code
> `cargo build`

#### Test the code
> `cargo test`

#### And Run the code
> `cargo run`

```
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/project_hello`
Hello, world!
$
```

#### Release the code with optimizaitons for production
> `cargo build --release`

## Next Steps
* We need some editor to write rust programs
* Most convenient editor is **vscode**; it has readymade extension for rust development;
* One recommended extension is rust-analyzer; it is also availble for other editors
* One can use his choice of editor like vim and emacs

Goto [[Home]] for tutorial
