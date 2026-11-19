<p align="center">
  <img src="https://diegopn.github.io/potoo-site/src/assets/images/language-interpreter-icon.svg?v=1c651ca7" alt="Potoo IDE language interpreter icon" width="120" />
</p>

<h1 align="center">Calango Interpreter</h1>

<p align="center">An independent Rust interpreter library and CLI for the Calango pseudolanguage.</p>

<p align="center">
  <a href="https://github.com/diegopn/potoo-calango-interpreter/releases/latest">Download</a> ·
  <a href="https://diegopn.github.io/potoo-site/">Website</a> ·
  <a href="https://diegopn.github.io/potoo-site/manual/calango/">User manual</a>
</p>

<p align="center">
  <a href="https://www.rust-lang.org/"><img alt="Rust" src="https://img.shields.io/badge/Rust-000000?style=for-the-badge&logo=rust&logoColor=white"></a>
  <a href="https://doc.rust-lang.org/cargo/"><img alt="Cargo" src="https://img.shields.io/badge/Cargo-DEA584?style=for-the-badge&logo=rust&logoColor=white"></a>
  <img alt="Rust library" src="https://img.shields.io/badge/Rust_library-4A86CF?style=for-the-badge">
  <img alt="CLI" src="https://img.shields.io/badge/CLI-6A4C93?style=for-the-badge">
</p>

Originally developed as part of the Potoo IDE project, this interpreter is an independent Rust software package intended to be distributed separately from the IDE. It can be embedded as a library in other Rust projects or run as a standalone command-line tool on Linux, macOS, and Windows.

It reads Calango’s `algoritmo`/`principal` structure and Portuguese keywords for typed variables, arrays, routines, expressions, control flow, and console I/O. The library exposes source-located diagnostics, host-provided input and output, cancellation, and debugger events. The built-in `pi()` returns Rust’s IEEE-754 binary64 approximation (`f64::consts::PI`) of π.

## Original language references

- [Calango 1.1 and the learning environment overview (UnB FGA)](https://sae.unb.br/cae/conteudo/unbfga/apc/new_ambientedealgoritmo.html)

The reference describes the original Calango environment and its role in teaching programming logic. This repository provides an independent Rust implementation.

## Original language references

- [Calango 1.1 and the learning environment overview (UnB FGA)](https://sae.unb.br/cae/conteudo/unbfga/apc/new_ambientedealgoritmo.html)

The reference describes the original Calango environment and its role in teaching programming logic. This repository provides an independent Rust implementation.

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

## Support

If this interpreter is useful in your work, you can support its continued development:

<p align="left">
  <a href="https://github.com/sponsors/diegopn"><img alt="Become a sponsor" src="https://img.shields.io/badge/Become_a_sponsor-EA4AAA?style=for-the-badge&logo=githubsponsors&logoColor=white"></a>
</p>

## Project policies

- [Code of conduct](CODE_OF_CONDUCT.md)
- [Contributing](CONTRIBUTING.md)
- [Security policy](SECURITY.md)

## Standalone command-line runner

The `potoo-calango` command runs one source file without Potoo IDE. For example:

```sh
potoo-calango example.clg
```

The terminal runner provides standard input/output. It continues through debugger events without an interactive debugger.

## Install a prebuilt runner

The v0.1.1 release provides native CLI packages for Linux x86_64 (GNU), Windows x86_64 (MSVC), Intel macOS x86_64, and Apple silicon macOS arm64. Download the archive for your system and `SHA256SUMS` from the [GitHub Releases page](https://github.com/diegopn/potoo-calango-interpreter/releases), then run the commands below from the folder containing both files. Each archive contains the `potoo-calango` executable and the repository license. Replace `example.clg` with the path to a source file; the release package does not include a sample file.

### Linux x86_64 (GNU)

```sh
set -euo pipefail
asset="potoo-calango-interpreter-v0.1.1-x86_64-unknown-linux-gnu.tar.gz"
grep -F "  $asset" SHA256SUMS | sha256sum --check -
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
export PATH="$HOME/.local/bin:$PATH"
potoo-calango example.clg
```

To keep the command available in new Linux sessions, add the `export PATH="$HOME/.local/bin:$PATH"` line to your shell profile.

### Windows x86_64 (MSVC, PowerShell)

```powershell
$archive = "potoo-calango-interpreter-v0.1.1-x86_64-pc-windows-msvc.zip"
$line = Get-Content .\SHA256SUMS | Where-Object { $_.EndsWith("  $archive") } | Select-Object -First 1
if (-not $line) { throw "No SHA256SUMS entry found for $archive" }
$expected = ($line -split '\s+')[0]
$actual = (Get-FileHash -LiteralPath $archive -Algorithm SHA256).Hash.ToLowerInvariant()
if ($actual -ne $expected) { throw "SHA-256 mismatch for $archive" }
$installDir = Join-Path $env:USERPROFILE ".local\bin"
New-Item -ItemType Directory -Force $installDir | Out-Null
Expand-Archive -LiteralPath $archive -DestinationPath $installDir
$exe = Join-Path $installDir "potoo-calango-interpreter-v0.1.1-x86_64-pc-windows-msvc\potoo-calango.exe"
& $exe .\example.clg
```

To run `potoo-calango.exe` from any folder, add `$env:USERPROFILE\.local\bin\potoo-calango-interpreter-v0.1.1-x86_64-pc-windows-msvc` to your user `Path` environment variable in Windows Settings.

### Intel macOS x86_64

```sh
set -euo pipefail
asset="potoo-calango-interpreter-v0.1.1-x86_64-apple-darwin.tar.gz"
grep -F "  $asset" SHA256SUMS | shasum -a 256 -c -
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
export PATH="$HOME/.local/bin:$PATH"
potoo-calango example.clg
```

### Apple silicon macOS arm64

```sh
set -euo pipefail
asset="potoo-calango-interpreter-v0.1.1-aarch64-apple-darwin.tar.gz"
grep -F "  $asset" SHA256SUMS | shasum -a 256 -c -
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
export PATH="$HOME/.local/bin:$PATH"
potoo-calango example.clg
```

On macOS, add the `export PATH="$HOME/.local/bin:$PATH"` line to `~/.zprofile` to keep the command available in future sessions.


## Install from source

With Rust 1.93 or later and network access, Cargo can install the command directly from its tagged source:

```sh
cargo install --git https://github.com/diegopn/potoo-calango-interpreter.git --tag v0.1.1 --bin potoo-calango
```

## Use as a Rust library

```toml
[dependencies]
potoo-calango-interpreter = { git = "https://github.com/diegopn/potoo-calango-interpreter.git", rev = "e81b43757d77c8bd59a39163c76003d76d7e0761" }
```

The library API lets Rust applications provide their own `RuntimeIo` implementation.

## DOI and citation

DOI: [10.6084/m9.figshare.34914153](https://doi.org/10.6084/m9.figshare.34914153)

Suggested reference (ABNT NBR 6023:2018):

> NASCIMENTO, Diego Pereira do. *Calango interpreter v0.1.0*. 2026. Programa de computador. Disponível em: https://doi.org/10.6084/m9.figshare.34914153. Acesso em: 4 nov. 2026.
