# claude-spade-lsp

A [Claude Code](https://claude.com/claude-code) plugin marketplace that registers
the [Spade HDL](https://spade-lang.org) language server for `.spade` files. With
it installed, Claude Code's built-in `LSP` tool can do go-to-definition, hover,
find-references, and document-symbol lookups on Spade code.

## What's in here

- `.claude-plugin/marketplace.json`: the marketplace manifest. It declares one
  plugin, `spade-lsp`, whose `lspServers` entry maps the `.spade` extension to
  the `spade-language-server` command.
- `plugins/spade-lsp/`: the plugin manifest and its README.

## Prerequisites

The `spade-language-server` binary has to be on your `PATH`. It is part of the
Spade compiler. Install it with:

```sh
cargo install --git https://gitlab.com/spade-lang/spade spade-language-server
```

That places it at `~/.cargo/bin/spade-language-server`, so make sure
`~/.cargo/bin` is on your `PATH`. If you already use
[`swim`](https://gitlab.com/spade-lang/swim), this is the same server that
`swim lsp` builds and runs; installing the standalone binary just skips
`swim lsp`'s first-run compile. See [Using `swim lsp` instead](#using-swim-lsp-instead).

## Install

```sh
claude plugin marketplace add Mechazawa/claude-spade-lsp
claude plugin install spade-lsp@spade
```

Then restart Claude Code (quit and relaunch, or "Reload Window" in the IDE).
Language servers are registered at startup, so the plugin does not take effect
until a restart.

## Verify

```sh
claude plugin list
```

`spade-lsp@spade` should be listed as enabled with one LSP server (`spade`).
Open a `.spade` file and ask Claude to look up a symbol to confirm it responds.

## Using `swim lsp` instead

If you would rather not install the standalone binary, point the server at
`swim lsp`, which builds `spade-language-server` on first use and then runs it.
In your fork, set `lspServers.spade` in `marketplace.json` to:

```json
"command": "swim",
"args": ["lsp"]
```

One caveat: on its first run in a project, `swim lsp` compiles the server and
prints build progress to stdout, which corrupts the LSP stream. Build it once
(`swim build`, or run `swim lsp` manually) before relying on the LSP, and again
after any `swim clean`.

## Notes

- The server operates on a swim project. Open or launch Claude Code from the
  directory containing `swim.toml` so the server can resolve the project.
- Claude Code loads `lspServers` only from git-cloned marketplaces, which is why
  this ships as a repository rather than a local directory.

## Uninstall

```sh
claude plugin uninstall spade-lsp@spade
claude plugin marketplace remove spade
```
