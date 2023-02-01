Checkout and build the [CloudTruth CLI](cloudtruth/cloudtruth-cli) from source.

- [Usage](#usage)
- [Inputs](#inputs)
- [Examples](#examples)

## Usage

Add to your GitHub Workflow to checkout the Cloudtruth CLI and build it from source with the Rust compiler. This action automatically installs the required Rust toolchain for you.

```yaml
    - uses: cloudtruth/cli-build-action@v1
```

By default, this will checkout the `main` branch of [cloudtruth/cloudtruth-cli](cloudtruth/cloudtruth-cli) into the workspace directory, install the Rust toolchain version specified by the `rust-toolchain` file present in the repo, build the CLI executable with Cargo, and run unit tests.

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

|     INPUT     |  TYPE  | REQUIRED |        DEFAULT        |                                                                                    DESCRIPTION                                                                                     |
|---------------|--------|----------|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| buildOptions  | string |  false   |                       |                                                              Command-line options passed directly to `cargo<br>build`                                                              |
|   cacheKey    | string |  false   | `"v0-cloudtruth-cli"` |                                                     A custom string to prefix cache<br>keys with when caching build artifacts                                                      |
|  components   | string |  false   |                       |                                                            Rust toolchain components to install (ex<br>rustfmt, clippy)                                                            |
|     path      | string |  false   |         `"."`         |                                                          Relative path under $GITHUB_WORKSPACE to place<br>the repository                                                          |
|      ref      | string |  false   |                       |                                                             Git ref to build from (defaults<br>to the default branch)                                                              |
|   skipCache   | string |  false   |                       |                                                                 Skip caching of Cargo registry and<br>dependencies                                                                 |
| skipUnitTests | string |  false   |                       |                                                                              Skips running unit tests                                                                              |
|    target     | string |  false   |                       | Build targets to install with `rustup`.<br> Note you will still need to<br> pass in the `--target` build option<br> with `buildOptions` if you want Cargo<br>to build this target) |
|  testOptions  | string |  false   |                       |                                                              Command-line options passed directly to `cargo<br>test`                                                               |

<!-- AUTO-DOC-INPUT:END -->


## Examples

Check out source in a child directory

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
          path: cloudtruth-cli
```

Check out a specifc branch/ref (same format as `actions/checkout`)

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
          ref: refs/tags/1.1.8
```

Add `cargo build` and `cargo test` options

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
          buildOptions: --release
          testOptions: --no-fail-fast
```

Install Rust toolchain with additional components

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
            components: rustfmt, clippy ## formatter and linter
```

Skip unit tests and build caching

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
          skipCache: true
          skipUnitTests: true
```

Custom build target (for cross-compilation). Note that `target` is only used to 
tell `rustup` to install `rust-std` bindings for that particular target, but
to compile the CLI for that target you'll still need to add the `--target` option to
your `buildOptions`.

```yaml
    - uses: cloudtruth/cli-build-action@v1
        with:
            ## Install rust-std for WebAssembly
            target: wasm32-unknown
            ## Build for Webassembly
            buildOptions: --target wasm32-unknown --release
```
