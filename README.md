# cargo-patch

`Cargo-Patch` is a Cargo Subcommand which allows
patching dependencies using patch files.

## Installation

Simply run:

```sh
cargo install cargo-patch
```

## Generating a patch using git

To generate a patch file of the current uncommited changes in a git repo
```
git diff > mypatch.patch
```

## Usage

To patch a dependency one has to add the following
to `Cargo.toml`:

```toml
[package.metadata.patch.serde]
version = "1.0"
patches = [
    "test.patch"
]
```

It specifies which dependency to patch (in this case
serde) and one or more patchfiles to apply. Running:

```sh
cargo patch
```

will download the serde package specified in the
dependency section to the `target/patch` folder
and apply the given patches. To use the patched
version one has to override the dependency using
`replace` like this

```toml
[patch.crates-io]
serde = { path = './target/patch/serde-1.0.110' }
```

## Patch format

You can either use [diff](http://man7.org/linux/man-pages/man1/diff.1.html) or
[git](https://linux.die.net/man/1/git) to create patch files. Important is that
file paths are relativ and inside the dependency

## Limitations

Its only possible to patch dependencies of binary crates as it is not possible
for a subcommand to intercept the build process.

