# Awesome Text-First Interfaces [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> Build the CLI/text protocol before the GUI. Justify the graphical interface.

A curated, decision-guide awesome-list on when a text-first interface (CLI, TUI, or a plain line-based protocol) beats a GUI — and when it genuinely doesn't. Not a plain link dump: the goal is to help you pick the right interface layer for a tool, not just browse frameworks.

## Introduction

A GUI is a strictly bigger commitment than a text interface: you're signing up for layout, rendering across platforms, accessibility, input handling, and a much larger design surface — before the tool does anything useful. A text-first interface (a CLI, a TUI, or a plain line-delimited protocol over stdio/a socket) is scriptable, composable with other tools via pipes, trivially remotable over SSH or a low-bandwidth link, easy to test (you can assert on stdout), and cheap to build incrementally. Most developer tools, and a growing number of AI agent interfaces, are better off starting there — and some never need to leave.

This isn't an anti-GUI list. It's a guide to sequencing: build the text interface first, because it forces a clean underlying API, and only add a GUI once you know it earns its cost.

**Licensing note:** every entry below is tagged with its license/status — `(MIT)`, `(Apache-2.0)`, etc. for genuine FOSS, or `(proprietary)` for closed-source products. A couple of proprietary tools are included as legitimate, important real-world examples of the text-first pattern; the tag tells you upfront whether you can actually read, fork, or self-host it, or whether it's a closed product you can only use as shipped.

## Contents

- [Decision Heuristics](#decision-heuristics)
- [CLI Frameworks](#cli-frameworks)
- [TUI Frameworks](#tui-frameworks)
- [Text & Line-Based Protocols](#text--line-based-protocols)
- [Text-First Tools That Scaled](#text-first-tools-that-scaled)
- [Text-First AI Agent Interfaces](#text-first-ai-agent-interfaces)
- [When You Actually Need a GUI](#when-you-actually-need-a-gui)
- [Further Reading](#further-reading)

## Decision Heuristics

Lean text-first when:
- The primary users are the people who'll also script/automate it.
- The tool needs to run headless, over SSH, in CI, or on a remote/low-bandwidth link.
- Composability matters — piping output into `jq`, `grep`, another tool, or another agent.
- You want the interface to double as an API for free (a well-structured CLI *is* an API).
- You're not sure yet what the right abstraction is — text interfaces are cheap to redesign.

Reach for a GUI when:
- The task is inherently spatial or visual: layout, diagramming, image/video editing, drawing.
- Users are non-technical and won't tolerate a command line, ever.
- You need rich, interactive data visualization that text/TUI rendering can't approximate well.
- Discoverability matters more than speed — GUIs surface affordances text hides behind `--help`.

## CLI Frameworks

- [Cobra](https://github.com/spf13/cobra) `(Apache-2.0)` — the Go CLI framework behind `kubectl`, `hugo`, `gh`, and much of the modern Go tooling ecosystem.
- [Click](https://github.com/pallets/click) `(BSD-3-Clause)` — Python's most widely used CLI framework, composable commands via decorators.
- [Typer](https://github.com/fastapi/typer) `(MIT)` — Python CLI framework built on type hints, from the FastAPI author.
- [Clap](https://github.com/clap-rs/clap) `(MIT/Apache-2.0)` — Rust's de facto CLI argument parser, derive-macro friendly.
- [yargs](https://github.com/yargs/yargs) `(MIT)` — the dominant Node.js CLI argument parser.
- [oclif](https://github.com/oclif/oclif) `(MIT)` — Salesforce/Heroku's framework for building large, plugin-based CLIs.

## TUI Frameworks

- [Bubble Tea](https://github.com/charmbracelet/bubbletea) `(MIT)` — Elm-architecture TUI framework in Go, paired with [Bubbles](https://github.com/charmbracelet/bubbles) `(MIT)` (components) and [Lip Gloss](https://github.com/charmbracelet/lipgloss) `(MIT)` (styling). Powers Crush, gh's interactive prompts, and much of the modern terminal-app wave.
- [Textual](https://github.com/Textualize/textual) `(MIT)` — Python TUI framework with a CSS-like styling model and a real widget system.
- [ratatui](https://github.com/ratatui/ratatui) `(MIT)` — Rust's immediate-mode TUI library, the successor to `tui-rs`.
- [blessed](https://github.com/chjj/blessed) `(MIT)` / [neo-blessed](https://github.com/embarklabs/neo-blessed) `(MIT)` — classic curses-style terminal UI toolkit for Node.js.
- [Ink](https://github.com/vadimdemedes/ink) `(MIT)` — React for CLIs; render TUIs with JSX and component state.

## Text & Line-Based Protocols

- [JSON-RPC 2.0](https://www.jsonrpc.org/specification) `(open spec)` — the lingua franca for text-first tool-to-tool communication; trivial to implement, trivial to debug on the wire.
- Newline-delimited JSON (NDJSON) `(open spec)` — the simplest possible streaming protocol: one JSON object per line, works over any pipe or socket.
- [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/) `(open spec, Microsoft-stewarded)` — JSON-RPC over stdio/sockets, proof that a text protocol can standardize an entire tooling ecosystem across editors.
- [Agent Client Protocol (ACP)](https://agentclientprotocol.com/) `(open spec)` — an emerging JSON-RPC-based protocol for editor↔agent communication, the same shape as LSP applied to AI coding agents.

## Text-First Tools That Scaled

- **git** `(GPL-2.0)` — no official GUI for over a decade of dominance; the CLI *is* the product, GUIs are third-party wrappers built on top of it.
- **Docker CLI** `(Apache-2.0)` — `docker` predates Docker Desktop by years; the CLI remains the primary interface for automation and CI (note: Docker Desktop itself is proprietary — the CLI and the underlying `moby`/`docker engine` are open source).
- **kubectl** `(Apache-2.0)` — Kubernetes' CLI came first and is still how most cluster automation happens; GUIs like k9s `(Apache-2.0)` and Lens `(MIT core, proprietary cloud features)` are optional lenses on top, not replacements.
- **psql** `(PostgreSQL License)` **/ mysql** `(GPL-2.0)` **/ sqlite3** `(public domain)` — database CLIs remain the fastest path for scripting, migrations, and ad-hoc queries even though every major database has GUI clients.

## Text-First AI Agent Interfaces

Coding agents are a live example of the same sequencing argument: a text/TUI interface over a well-defined protocol lets the agent be scripted, run headless in CI, and driven remotely — while a GUI can be layered on top later without changing the underlying contract.

- [opencode](https://github.com/anomalyco/opencode) `(MIT)` — TUI-first coding agent with a headless server (`opencode serve`) underneath; the TUI is just one client of a text/HTTP protocol, with web and desktop clients layered on afterward. Fully open source, buildable and self-hostable end to end.
- [Aider](https://github.com/Aider-AI/aider) `(Apache-2.0)` — terminal-first AI pair-programming tool, git-integrated, scriptable from day one. Fully open source.
- [Claude Code](https://github.com/anthropics/claude-code) `(proprietary)` — CLI/TUI-first agent from Anthropic; the same core is also embeddable and drivable programmatically. Included as a genuinely important real-world example of the text-first pattern, but note it's closed-source — the linked repo is issue-tracking/releases for a distributed binary, not a buildable source tree. If you want to study or modify the implementation rather than just use it, opencode and Aider above are the ones you can actually do that with.

## When You Actually Need a GUI

Be honest with yourself here — this list isn't dogma:
- Image, video, or vector editing — there's no text-first substitute for direct spatial manipulation.
- Consumer-facing products where the user will never open a terminal.
- Dashboards meant for continuous ambient monitoring by non-technical stakeholders.
- Complex multi-pane debugging/profiling tools where visual correlation across panes beats scrollback.

## Further Reading

- ["The Art of Unix Programming" — Eric S. Raymond](http://www.catb.org/~esr/writings/taoup/html/) — the philosophical foundation for composable, text-based tools.
- ["Rule of Composition"](https://en.wikipedia.org/wiki/Unix_philosophy) — programs that do one thing and talk in text compose better than programs that don't.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](LICENSE)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work. See [LICENSE](LICENSE).
