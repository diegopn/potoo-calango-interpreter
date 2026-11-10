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

Prebuilt binaries are published on the [GitHub Releases page](https://github.com/diegopn/potoo-calango-interpreter/releases). Choose a version and the package for your system there. Packages are currently available for Linux x86_64, Windows x86_64 (MSVC), and Intel macOS (x86_64). The current release does not include an Apple silicon (arm64) package.

Download only the package for your system and the `SHA256SUMS` file, and save both in the same folder. Before extracting or running the executable, compare the package's SHA-256 hash with its corresponding entry in `SHA256SUMS`. Run the commands below from that folder. Replace `example.clg` with the path to a source file you want to run; the release package does not include a sample file.

### Linux x86_64

```sh
asset=$(ls -1 potoo-calango-interpreter-v*-x86_64-unknown-linux-gnu.tar.gz | head -n 1)
sha256sum "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
export PATH="$HOME/.local/bin:$PATH"
potoo-calango example.clg
```

To keep the command available in new Linux sessions, add the `export PATH="$HOME/.local/bin:$PATH"` line to your shell profile.

### Windows x86_64 (PowerShell)

```powershell
$archive = Get-ChildItem -Filter "potoo-calango-interpreter-v*-x86_64-pc-windows-msvc.zip" | Select-Object -First 1
Get-FileHash $archive.FullName -Algorithm SHA256 # compare with the matching SHA256SUMS entry
Expand-Archive $archive.FullName -DestinationPath .\potoo-calango
$exe = Get-ChildItem .\potoo-calango -Filter "potoo-calango.exe" -Recurse | Select-Object -First 1
& $exe.FullName .\example.clg
```

To run `potoo-calango.exe` from another folder, add the directory containing the executable to your user `PATH`.

### Intel macOS

The published macOS package targets Intel Macs (x86_64). There is no prebuilt Apple silicon package in the current release.

```sh
asset=$(ls -1 potoo-calango-interpreter-v*-x86_64-apple-darwin.tar.gz | head -n 1)
shasum -a 256 "$asset" # compare with its line in SHA256SUMS
tar -xzf "$asset"
mkdir -p "$HOME/.local/bin"
install -m 755 "${asset%.tar.gz}/potoo-calango" "$HOME/.local/bin/potoo-calango"
export PATH="$HOME/.local/bin:$PATH"
potoo-calango example.clg
```

Add the `export PATH="$HOME/.local/bin:$PATH"` line to `~/.zprofile` to keep the command available in future macOS sessions.


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

## DOI and citation

DOI: [10.6084/m9.figshare.34914153](https://doi.org/10.6084/m9.figshare.34914153)

Suggested reference (ABNT NBR 6023:2018):

> NASCIMENTO, Diego Pereira do. *Calango interpreter v0.1.0*. 2026. Programa de computador. Disponível em: https://doi.org/10.6084/m9.figshare.34914153. Acesso em: 4 nov. 2026.
