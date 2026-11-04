# potoo-calango-interpreter

An independent Rust interpreter for the Calango pseudolanguage. It reads the
`algoritmo`/`principal` structure and Calango's Portuguese keywords, including
typed variables, arrays, routines, expressions, control flow, and console I/O.

This is a library crate for embedding Calango execution in a Rust application;
it includes a standalone terminal command-line runner. It exposes
source-located diagnostics, host-provided input and output, cancellation, and
debugger events.

The Calango built-in `pi()` returns Rust's IEEE-754 binary64 approximation
(`f64::consts::PI`) of π.

## Source example

```clg
algoritmo exemplo;
principal
  escreval("Olá, Calango!");
fimPrincipal
```

## Use from a Rust project

Install Rust 1.93 or later and add the pinned Git dependency below to your application's `Cargo.toml`.

```toml
[dependencies]
potoo-calango-interpreter = { git = "https://github.com/diegopn/potoo-calango-interpreter.git", rev = "e81b43757d77c8bd59a39163c76003d76d7e0761" }
```

Implement `RuntimeIo` to provide input and output, then call `execute(source,
&mut io)`. The public API and host callbacks are documented in the crate
source. For a local checkout, run the interpreter's tests with `cargo test`.

## Development

Run these commands from the repository root:

```sh
cargo fmt --all -- --check
cargo check
cargo test
```

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).

## Project policies

- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Standalone command-line runner

The `potoo-calango` command runs one source file without Potoo IDE. For example:

```sh
potoo-calango example.clg
```

The terminal runner provides standard input/output. It continues through debugger events without an interactive debugger.

## Install a prebuilt runner

Download the archive for your system and `SHA256SUMS` from the [latest GitHub Release](https://github.com/diegopn/potoo-calango-interpreter/releases/latest). Binary releases are not published yet. The planned first release will be tagged `v0.1.0`; its release workflow is configured to produce these asset names:

| System | Archive |
| --- | --- |
| Linux x86_64 | `potoo-calango-interpreter-v0.1.0-x86_64-unknown-linux-gnu.tar.gz` |
| Windows x86_64 | `potoo-calango-interpreter-v0.1.0-x86_64-pc-windows-msvc.zip` |
| macOS Intel | `potoo-calango-interpreter-v0.1.0-x86_64-apple-darwin.tar.gz` |


Current CI and release targets are Linux x86_64, Windows x86_64, and macOS Intel (x86_64). Apple silicon (arm64) is not supported in this release; support may be added in a future version.

### Linux x86_64

```sh
asset=potoo-calango-interpreter-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
release=https://github.com/diegopn/potoo-calango-interpreter/releases/download/v0.1.0
curl -fLO "$release/$asset"
curl -fLO "$release/SHA256SUMS"
sha256sum "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "potoo-calango-interpreter-v0.1.0-x86_64-unknown-linux-gnu/potoo-calango" "$HOME/.local/bin/potoo-calango"
potoo-calango example.clg
```

Add it for the current shell with `export PATH="$HOME/.local/bin:$PATH"`; place that line in your shell profile to keep it for future sessions.

### Windows x86_64 (PowerShell)

```powershell
$asset = "potoo-calango-interpreter-v0.1.0-x86_64-pc-windows-msvc.zip"
$release = "https://github.com/diegopn/potoo-calango-interpreter/releases/download/v0.1.0"
Invoke-WebRequest "$release/$asset" -OutFile $asset
Invoke-WebRequest "$release/SHA256SUMS" -OutFile SHA256SUMS
Get-FileHash ".\$asset" -Algorithm SHA256 # compare with the matching SHA256SUMS entry
Expand-Archive ".\$asset" -DestinationPath .\potoo-calango-interpreter
.\potoo-calango-interpreter\potoo-calango-interpreter-v0.1.0-x86_64-pc-windows-msvc\potoo-calango.exe .\example.clg
```

To run `potoo-calango.exe` from any folder, add its extracted directory to your user `PATH` in Windows Environment Variables.

### macOS Intel

This release supports Intel Macs only. Apple silicon is not supported in this version.

```sh
asset=potoo-calango-interpreter-v0.1.0-x86_64-apple-darwin.tar.gz
release=https://github.com/diegopn/potoo-calango-interpreter/releases/download/v0.1.0
curl -fLO "$release/$asset"
curl -fLO "$release/SHA256SUMS"
shasum -a 256 "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
potoo-calango example.clg
```

Add it for the current shell with `export PATH="$HOME/.local/bin:$PATH"`; place that line in `~/.zprofile` to keep it for future macOS sessions.

## Install from source

With Rust 1.93 or later and network access, Cargo can install the command directly from its tagged source:

```sh
cargo install --git https://github.com/diegopn/potoo-calango-interpreter.git --rev e81b43757d77c8bd59a39163c76003d76d7e0761 --bin potoo-calango
```

## Use as a Rust library

```toml
[dependencies]
potoo-calango-interpreter = { git = "https://github.com/diegopn/potoo-calango-interpreter.git", rev = "e81b43757d77c8bd59a39163c76003d76d7e0761" }
```

The library API lets Rust applications provide their own `RuntimeIo` implementation.

## DOI

A stable Figshare DOI will be added when the first public release is published.
