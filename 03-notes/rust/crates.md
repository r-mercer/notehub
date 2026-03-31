# Crates and Modules

Rust uses a module system, with the outline (from the Rust Book) being as so:
    Packages: A Cargo feature that lets you build, test, and share crates
    Crates: A tree of modules that produces a library or executable
    Modules and use: Let you control the organization, scope, and privacy of paths
    Paths: A way of naming an item, such as a struct, function, or module

## Crates

Crates come in two flavours, binary crates and library crates. 

### Binary Crates

Binary crates are regular programs that compile into an executable and run, such as a command line or server. 

Each Binary crate has a main function, which executes the program.

### Library Crates

Library crates don't have a main function and don't compile into an executable. 

They provide additional functionality that are to be shared between multiple other projects.

## Packages

Packages are bundles of crates. They must contain the `cargo.toml` file, which describes how to build a crate. 

Packages can contain multiple binary crates, but only a single library crate.

## Modules, Scope and Privacy

From the perspective of the compiler, modules work like this:

- Start in the root
    - Usually `src/lib.rs` (for library crates) or `src/main.rs` (for binary crates)
- Check for declared modules
    - Declared in the above files as `mod {name}`
    - It will then find the module code in:
        - inline, declared within curlies after mod name declaration
        - in the file `src/{name}.rs`
        - in the file `src/{name}/mod.rs`
- Then check for sub-modules
    - Once it is pointed at a module, it will find sub-modules, as declared in the module
    - It looks in the following:
        - inline, declared within curlies after mod name declaration
        - in the file `src/{name}/{sub}.rs`
        - in the file `src/{name}/{sub}/mod.rs`
- Then it finds the actual code and can be referred to elsewhere
    - if you declare it `pub` you can find it anywhere, where as `mod` are private
- Once you are ready to use your module
    - just call use, then it will auto complete the namespace so you can use the objects in code without calling manually
