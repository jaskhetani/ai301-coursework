# Reproduction report for Lind #375

## Environment

- Microsoft Windows 11 Pro 10.0.26100
- Git 2.51.0.windows.1
- Python 3.13.7
- Fork checkout at upstream commit `908cb5a93b511f1db62e20aab4975f4401f7c6d6`
- Sparse checkout of `docs/` and `.github/`; this avoids unrelated case-colliding vendored Linux headers on Windows.

## Steps

1. From the clone root, run:

   ```text
   git grep -n -i -E "constant|crate-wide|project-wide|file-local" -- docs/contribute/styleguide.md
   ```

2. Observe that the command prints no matches.
3. In any shell, run:

   ```text
   git show HEAD:docs/contribute/styleguide.md
   ```

## Evidence

The relevant content of the current page contains only the default Rust style link and two `cargo fmt` commands:

````markdown
# Rust Style Guide

Rust code in `lind-wasm` follows the [default Rust style](https://doc.rust-lang.org/style-guide/index.html) and should be auto-formatted:

```bash
cargo fmt --all --manifest-path src/wasmtime/Cargo.toml
cargo fmt --all --manifest-path src/lind-boot/Cargo.toml
```
````

**Expected:** The style guide explains where local, crate-wide, and project-wide shared constants belong.

**Actual:** The page covers the default Rust style and `cargo fmt`, with no constant-placement guidance.

The targeted search returns no output, directly showing the missing contributor guidance. This reproduces the documentation gap described in issue #375; it does not test runtime behavior. The existing style-guide page is the smallest likely location because `CONTRIBUTING.md` already links to it, while a new page would also require navigation changes.
