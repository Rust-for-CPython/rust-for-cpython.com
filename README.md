# Rust for CPython Website

Rust for CPython is a project dedicated to working on a proposal to introduce Rust to the CPython project, the reference implementation of Python. This repository contains the source code for the [rust-for-cpython.com](https://rust-for-cpython.com) website.

The website is built using [mdBook](https://github.com/rust-lang/mdBook).

## Prerequisites

To build this book locally, you need to have `mdbook` installed. If you have Rust and `cargo` installed, you can install it via:

```bash
cargo install mdbook
```

## Local Development

To serve the book locally and have it automatically reload when you make changes, run:

```bash
mdbook serve --open
```

This will start a local server (usually at `http://localhost:3000`) and open it in your default browser.

## Building

To build the book and generate the HTML files in the `book/` directory:

```bash
mdbook build
```

## Contributing

Please see [src/contributing.md](src/contributing.md) for more information on how to contribute.
