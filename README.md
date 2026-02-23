# nuls (fork)

> **This is a fork of the original [nuls](https://github.com/cesarferreira/nuls) by [cesarferreira](https://github.com/cesarferreira).**
>
> The original `nuls` is an incredibly well-crafted, beautifully designed CLI tool that brings NuShell's elegant table-based `ls` output to any terminal. Huge thanks to **cesarferreira** for creating such an amazing developer tool that inspired this fork!

This fork adds **tree view mode**, **interactive pagination**, **file owner display**, and **enhanced color control** while preserving everything that makes the original nuls great.

**Forked from:** [github.com/ozenalp22/nuls](https://github.com/ozenalp22/nuls)
**This fork:** [github.com/HNKNTA/nuls](https://github.com/HNKNTA/nuls)

---

![Crates.io](https://img.shields.io/crates/v/nuls)
![License](https://img.shields.io/badge/license-MIT-blue)
![Tests](https://img.shields.io/badge/tests-pass-brightgreen)

<p align="center">
  <img src="public/screenshot.png" alt="nuls screenshot" width="800">
</p>

NuShell-inspired `ls` with a colorful, table-based layout: directory/file type tagging, human-readable sizes, relative "modified" times with recency-driven colors, and familiar flags.

---

## What's New in This Fork

### Tree View Mode (`--tree`)
Recursive directory listing in a beautiful table format with tree connectors:

```
┌────┬────────────────────────┬──────┬───────┬───────────────┐
│  # │ name                   │ type │  size │ modified      │
├────┼────────────────────────┼──────┼───────┼───────────────┤
│  0 │ ├── config             │ dir  │ 128 B │ 7 seconds ago │
│  1 │ │   ├── local          │ dir  │  64 B │ 7 seconds ago │
│  2 │ │   └── production     │ dir  │  64 B │ 7 seconds ago │
│  3 │ ├── projects           │ dir  │ 128 B │ 7 seconds ago │
│  4 │ │   ├── api-server     │ dir  │ 128 B │ 7 seconds ago │
│  5 │ │   │   ├── middleware │ dir  │  64 B │ 7 seconds ago │
│  6 │ │   │   └── routes     │ dir  │  64 B │ 7 seconds ago │
│  7 │ │   └── web-app        │ dir  │  96 B │ 7 seconds ago │
│  8 │ │       └── src        │ dir  │ 128 B │ 7 seconds ago │
│  9 │ └── tests              │ dir  │ 128 B │ 7 seconds ago │
│ 10 │     ├── integration    │ dir  │  96 B │ 7 seconds ago │
│ 11 │     └── unit           │ dir  │  64 B │ 7 seconds ago │
└────┴────────────────────────┴──────┴───────┴───────────────┘
```

### Interactive Pagination (`-n`)
Limit results with `Enter` to continue, auto-exits when done (no `q` needed):

```
│ 18 │ ├── project-18         │ dir  │ 128 B │ 1 day ago     │
│ 19 │ └── project-19         │ dir  │  96 B │ 2 days ago    │
-- 81 more, press Enter to continue --
```

### File Owner Column (`-l`)
Show the file owner in a dedicated column:

```
┌───┬────────────┬────────┬──────┬───────┬───────────────┐
│ # │ name       │ owner  │ type │  size │ modified      │
├───┼────────────┼────────┼──────┼───────┼───────────────┤
│ 0 │ src        │ user   │ dir  │ 160 B │ 2 minutes ago │
│ 1 │ tests      │ user   │ dir  │  96 B │ 1 hour ago    │
│ 2 │ Cargo.toml │ user   │ file │ 220 B │ 5 minutes ago │
└───┴────────────┴────────┴──────┴───────┴───────────────┘
```

### Color Control (`--color`)
Control ANSI color output for clean clipboard copying:
- `--color=auto` — colors when terminal, plain when piped (default)
- `--color=always` — force colors
- `--color=never` — no colors (clean text for clipboard)

---

## Fork Features

| Feature | Command | Description |
|---------|---------|-------------|
| Tree view | `--tree` | Recursive directory listing with tree connectors |
| Depth limit | `-d, --depth N` | Limit tree recursion depth |
| Dirs only | `-D, --dirs-only` | Show only directories (hide files) |
| Owner column | `-l, --long` | Show file owner username |
| Pagination | `-n, --limit N` | Show N results, press Enter for more |
| Color control | `--color=never` | Plain output for clipboard copying |

---

## Command Examples

### Basic Tree View
```bash
nuls --tree -d 2
```
Shows all files and directories up to 2 levels deep.

### Directories Only (Sorted by Modified)
```bash
nuls --tree -d 2 -t -D
```
Shows only directories, sorted by most recently modified.

### Paginated Output
```bash
nuls --tree -d 2 -t -D -n 20
```
Shows 20 results at a time, press Enter to see more.

### Copy to Clipboard (Clean Text)
```bash
nuls --tree -d 2 --color=never | pbcopy
```
Copies plain text (no ANSI codes) to clipboard on macOS.

### Show + Copy Combo
```bash
nuls --tree -d 1 -t -D && nuls --tree -d 1 -t -D --color=never | pbcopy
```
Displays colored output AND copies clean text to clipboard.

---

## Recommended Aliases

```bash
# Quick folder overview (depth 1, dirs only, sorted by modified, copies to clipboard)
alias folder="nuls --tree -d 1 -t -D -n 20 && nuls --tree -d 1 -t -D -n 20 --color=never | pbcopy"

# Deeper folder overview (depth 2)
alias folders="nuls --tree -d 2 -t -D -n 20 && nuls --tree -d 2 -t -D -n 20 --color=never | pbcopy"

# Replace ls entirely
alias ls="nuls"
```

---

## Original Features

- Box-drawn table with colored borders and headers
- Directory-first sorting by default; optional `-t/--sort-modified` (newest first) and `-r/--reverse`
- Relative modified column with recency-aware colors (seconds → years, plus future)
- Human-readable sizes (`KB`, `MB`, `GB`, `TB`)
- Hidden files toggled via `-a/--all`
- Colored help output for quick scanning
- Optional git info (`-g`) shown inline after the name, e.g., `main.rs (+15 -2)`

## Install

Building locally:
```bash
cargo install --path . --bin nuls --force
```

## All Flags

| Flag | Description |
|------|-------------|
| `-a, --all` | Show dotfiles |
| `-l, --long` | Show file owner column |
| `-t, --sort-modified` | Sort by modified time (newest first) |
| `-r, --reverse` | Reverse sort order |
| `-g, --git` | Show git status inline (+added/-deleted) |
| `--tree` | Enable tree view (recursive listing) |
| `-d, --depth N` | Max depth for tree mode |
| `-D, --dirs-only` | Show only directories |
| `-n, --limit N` | Limit/paginate results |
| `--color=always/auto/never` | Control ANSI color output |

## Palette

- Borders/header: teal/green highlights
- Names: dirs blue, files light gray, executables red, dotfiles amber, config/docs yellow
- Modified: green → yellow → orange → red → gray as timestamps get older; blue for future

## Credits

- **Original Author:** [cesarferreira](https://github.com/cesarferreira)
- **Original Repository:** [github.com/cesarferreira/nuls](https://github.com/cesarferreira/nuls)
- **Forked from:** [github.com/ozenalp22/nuls](https://github.com/ozenalp22/nuls)
- **This fork:** [github.com/HNKNTA/nuls](https://github.com/HNKNTA/nuls)
- **License:** MIT

---

*This fork was created with love for the terminal and deep appreciation for cesarferreira's excellent work on the original nuls.*