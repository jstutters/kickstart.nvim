# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal Neovim configuration based on [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim). The config lives at `~/.config/nvim` (this repo). Plugins are managed by `lazy.nvim`.

## Formatting

Lua files are formatted with `stylua` (installed via Mason). Config is in `.stylua.toml`:
- 2-space indentation, 160-column width, single quotes, no call parentheses on standalone calls.

Format a file: `stylua <file>.lua`

## Architecture

### Entry point
`init.lua` — single file (~1000 lines) containing all core options, keymaps, autocommands, and plugin specs. This is intentional; kickstart is designed to be readable top-to-bottom.

### Plugin directories
- `lua/kickstart/plugins/` — optional kickstart-provided plugins (debug, gitsigns, neo-tree, indent_line, lint, autopairs, none-ls). Each file returns a lazy.nvim plugin spec.
- `lua/custom/plugins/` — personal additions. Drop a `.lua` file here returning a plugin spec; it's auto-imported via `{ import = 'custom.plugins' }` in `init.lua`.

### Active optional plugins (enabled in `init.lua`)
- `kickstart.plugins.debug` — DAP debugging for Python and Go
- `kickstart.plugins.neo-tree` — file tree (`\` to toggle)
- `kickstart.plugins.gitsigns` — git hunk keymaps

### Key plugins configured in `init.lua`
| Plugin | Purpose |
|--------|---------|
| `telescope.nvim` | Fuzzy finder; `<leader><leader>` finds files |
| `nvim-lspconfig` + `mason` | LSP; servers: `pyright`, `rust_analyzer`, `lua_ls` |
| `blink.cmp` | Completion (sources: LSP, path, buffer, snippets, lazydev, copilot) |
| `copilot.lua` | GitHub Copilot (completions via blink-copilot) |
| `oil.nvim` | File explorer buffer (`<leader>o`) |
| `lazygit.nvim` | Git TUI (`<leader>lg`) |
| `nvim-dap` + `dapui` | Debugger UI (Python + Go adapters) |
| `mini.nvim` | Textobjects (ai), surround, statusline |
| `dracula.nvim` | Colorscheme |

## Adding plugins

**In-line (core):** add a spec directly inside `require('lazy').setup({...})` in `init.lua`.

**Modular (recommended for personal additions):** create `lua/custom/plugins/<name>.lua` returning a lazy.nvim spec table. It will be picked up automatically.

**Enabling kickstart optional plugins:** uncomment the relevant `require 'kickstart.plugins.<name>'` line near the bottom of `init.lua`.

## Key keymaps reference

`<Space>` is the leader key.

| Key | Action |
|-----|--------|
| `<leader><leader>` / `<leader>sf` | Find files (Telescope) |
| `<leader>sg` | Live grep |
| `<leader>sb` | Search open buffers |
| `<leader>sn` | Search Neovim config files |
| `<leader>sh` | Search help tags |
| `<leader>lg` | LazyGit |
| `<leader>o` | Oil file explorer |
| `\` | Neo-tree reveal |
| `<F5>` | DAP: start/continue |
| `<F1>/<F2>/<F3>` | DAP: step into/over/out |
| `<F7>` | DAP UI toggle |
| `<leader>b` | Toggle breakpoint |
| `grn` / `gra` / `grd` / `grr` | LSP: rename / code action / definition / references |
| `<leader>th` | Toggle inlay hints |
| `<C-h/j/k/l>` | Window navigation |
