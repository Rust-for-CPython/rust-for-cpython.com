# Building

This page covers what Rust adds to building CPython. It assumes you already
know how to build CPython itself — if not, start with the
[CPython Developer's Guide](https://devguide.python.org/getting-started/setup-building/).
Everything below describes only the *delta* the Rust integration introduces.

## Overview

The fork adds a [Cargo](https://doc.rust-lang.org/cargo/) workspace at the
repository root (`Cargo.toml`). It contains the `cpython-sys` bindings crate,
the `_base64` extension module, and supporting crates
(`cpython-rust-staticlib`, `cpython-build-helper`). These are compiled by
`cargo` as part of the normal CPython build, using the same module machinery
as the C extensions.

There are two build paths, selected automatically based on how extension
modules are built:

- **Shared module builds** — the default on Linux and macOS — compile each Rust
  module to a `cdylib` shared library (for example `_base64.so`) that is loaded
  at runtime, exactly like a C extension module.
- **Static module builds** aggregate the Rust modules into
  `cpython-rust-staticlib`, a single static archive that is linked into the
  interpreter binary. This is used when extension modules are built statically
  (for example WebAssembly targets).

Rust is **optional**. If `cargo` cannot be found, the build configures with a
warning and the Rust modules are simply disabled; CPython itself still builds
and runs. You only need the Rust toolchain when you want the Rust-based
extensions (such as `_base64`).

## Prerequisites

Beyond the usual CPython build dependencies, the Rust build needs:

- **A Rust toolchain**, installed via [`rustup`](https://rustup.rs/). The
  repository pins the toolchain in `rust-toolchain.toml` (currently `1.91.1`,
  with the `rustfmt` and `clippy` components). With `rustup` installed, the
  pinned version is fetched automatically the first time `cargo` runs in the
  tree — you do not need to select it by hand.
- **libclang.** The `cpython-sys` crate uses
  [`bindgen`](https://github.com/rust-lang/rust-bindgen) to generate bindings to
  CPython's C API, and `bindgen` requires libclang at build time. Install it
  through your platform's LLVM/Clang package (for example, `libclang-dev` on
  Debian/Ubuntu, or the LLVM formula on Homebrew).

## Unix and Linux

The standard flow needs no extra steps:

```console
$ ./configure
$ make
```

During `configure`:

- `cargo` is detected from `CARGO_HOME` (defaulting to `~/.cargo`). If it is
  missing, you get a warning and the Rust components are skipped.
- The Rust target triple is derived automatically from the platform. If
  `configure` cannot determine it, set `CARGO_TARGET` explicitly, for example:

  ```console
  $ ./configure CARGO_TARGET=x86_64-unknown-linux-gnu
  ```

- `--enable-optimizations` selects the `release` Cargo profile. Otherwise the
  `dev` profile is used, matching a regular debug build.

`make` then builds the Rust modules alongside the rest of CPython. On a
default (shared) build this produces the module shared libraries such as
`_base64.so`; the `cpython-rust-staticlib` archive is only produced for static
module builds.

`make clean` also runs `cargo clean` to remove the Rust build artifacts.

## macOS

The Unix flow above applies unchanged. The build handles the
platform-specific details for you: it uses the `.dylib` shared-library suffix
that Cargo expects on Apple platforms, and it propagates
`IPHONEOS_DEPLOYMENT_TARGET` when cross-compiling for iOS.

## Windows

The Visual Studio / `PCbuild` build builds the Rust extension through the
`_base64` project (`PCbuild/_base64.vcxproj`), which shells out to `cargo` with
the MSVC target triple matching the selected platform (for example
`x86_64-pc-windows-msvc` for `x64`).

Make sure `cargo` is available on `PATH` before building — for example by
running the build from a shell where `rustup`'s environment is active. If
`cargo` is **not** found on `PATH`, the `_base64` project is skipped with a
warning rather than failing the build, mirroring the Unix behavior.

```bat
PCbuild\build.bat
```

## Sanitizer and advanced builds

CPython's usual sanitizer `configure` flags still work:

- `--with-address-sanitizer`
- `--with-memory-sanitizer`
- `--with-undefined-behavior-sanitizer`
- `--with-thread-sanitizer`

Be aware that these flags instrument the **C** build only. The sanitizer
options add `-fsanitize=…` to the C compiler and linker flags, but nothing
propagates them into the Rust build — the Rust crates are currently compiled
**without** sanitizer instrumentation and then linked into the sanitized
interpreter. In other words, sanitizers cover CPython's C code, not the Rust
code, and the "sanitizers + Rust" combination is not yet exercised in CI.

## Verifying the Rust build

After a successful build, confirm the Rust extension loaded by importing it
from the freshly built interpreter:

```console
$ ./python -c "import _base64; print(_base64)"
```

If the import succeeds, the Rust components were built and linked correctly.
(On Windows, run the interpreter produced under `PCbuild` instead of
`./python`.)
