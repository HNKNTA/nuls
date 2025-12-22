# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

nuls is a NuShell-inspired `ls` replacement written in Rust. It displays directory listings in a colorful, box-drawn table format with human-readable sizes, relative timestamps with recency-aware colors, and optional inline git status.

## Build & Development Commands

```bash
# Build
cargo build

# Run tests
cargo test

# Run a single test
cargo test test_name

# Check without building
cargo check

# Install locally
cargo install --path . --bin nuls --force

# Run directly
cargo run -- [FLAGS] [PATH]
```

## Architecture

Single-file CLI application (`src/main.rs`) structured as:

- **CLI parsing**: Uses `clap` with derive macros. The `Cli` struct defines flags (`-a`, `-t`, `-r`, `-g`, `-l`)
- **Entry collection** (`collect_entries`): Reads directory, builds `EntryRow` structs with both plain and colored versions of each field for proper column width calculation
- **Sorting** (`sort_rows`): Default is directories-first + alphabetical; `-t` sorts by modified time (newest first); `-r` reverses
- **Git integration** (`load_git_info`, `read_git_status`, `merge_numstat`): Runs `git status --porcelain=1` and `git diff --numstat HEAD`, then scopes results to the listed directory via `scope_git_entries`
- **Rendering** (`render_table`): Box-drawing table with dynamic column widths, using separate plain/colored strings to compute visual width correctly
- **Color palette** (`mod palette`): ANSI 256-color codes for all UI elements; `paint()` helper wraps text with color codes

## Key Design Decisions

- Each `EntryRow` stores both plain text (for width calculation) and colored text (for display) because ANSI codes would break alignment otherwise
- Recency buckets (JustNow → Years) drive both the display text ("2 hours ago") and the color gradient
- Git status is aggregated per top-level entry when listing directories containing nested changes
