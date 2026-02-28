---
title: Text Editor Configuration
description: Configure Neovim and other text editors on Ubuntu — installation, configuration files, shortcuts, plugins, and best practices.
category: text-editors
order: 4
---

## Overview

Ubuntu offers a variety of text editors, from simple terminal-based tools like `nano` to powerful, extensible editors like Neovim. This guide focuses primarily on **Neovim** — the modern fork of Vim — while also covering other popular options and the key configuration files that control your Ubuntu environment.

## Installing Neovim

```bash
# Install from Ubuntu repos
sudo apt install neovim

# Or install the latest stable via PPA
sudo add-apt-repository ppa:neovim-ppa/stable
sudo apt update && sudo apt install neovim

# Or install the latest nightly via PPA
sudo add-apt-repository ppa:neovim-ppa/unstable
sudo apt update && sudo apt install neovim

# Verify installation
nvim --version
```

> **Tip:** If you want to use Neovim as your default editor system-wide, run:
> `sudo update-alternatives --set editor /usr/bin/nvim`

## Neovim Configuration Structure

Neovim uses the **XDG Base Directory** standard. Your configuration lives in `~/.config/nvim/`:

```
~/.config/nvim/
├── init.lua              # Main entry point (or init.vim for Vimscript)
├── lua/
│   ├── config/
│   │   ├── options.lua   # Editor options (line numbers, tabs, etc.)
│   │   ├── keymaps.lua   # Custom key mappings
│   │   ├── autocmds.lua  # Auto-commands (format on save, etc.)
│   │   └── lazy.lua      # Plugin manager bootstrap
│   └── plugins/
│       ├── lsp.lua       # LSP configuration
│       ├── treesitter.lua # Syntax highlighting
│       ├── telescope.lua  # Fuzzy finder
│       ├── completion.lua # Autocompletion
│       └── ui.lua         # Theme, statusline, etc.
├── after/
│   └── ftplugin/         # Filetype-specific overrides
│       ├── python.lua
│       ├── javascript.lua
│       └── markdown.lua
└── snippets/             # Custom code snippets
```

## Essential Neovim Options

Add these to your `lua/config/options.lua`:

```lua
local opt = vim.opt

-- Line numbers
opt.number = true           -- Show line numbers
opt.relativenumber = true   -- Relative line numbers (great for motions)

-- Indentation
opt.tabstop = 2             -- Tab width
opt.shiftwidth = 2          -- Indent width
opt.expandtab = true        -- Use spaces instead of tabs
opt.smartindent = true      -- Auto-indent new lines

-- Search
opt.ignorecase = true       -- Case-insensitive search
opt.smartcase = true        -- Case-sensitive if uppercase present
opt.hlsearch = false        -- Don't highlight all matches
opt.incsearch = true        -- Incremental search

-- Appearance
opt.termguicolors = true    -- True color support
opt.signcolumn = "yes"      -- Always show sign column
opt.cursorline = true       -- Highlight current line
opt.scrolloff = 8           -- Keep 8 lines above/below cursor
opt.wrap = false            -- No line wrapping

-- Splits
opt.splitbelow = true       -- Horizontal splits open below
opt.splitright = true       -- Vertical splits open right

-- System
opt.clipboard = "unnamedplus"  -- Use system clipboard
opt.undofile = true            -- Persistent undo history
opt.swapfile = false           -- No swap files
opt.updatetime = 250           -- Faster CursorHold events
```

## Neovim Modes

Neovim is a **modal editor** — understanding modes is fundamental:

| Mode | Key to Enter | Purpose |
|------|-------------|---------|
| **Normal** | `Esc` | Navigate, delete, copy, paste, run commands |
| **Insert** | `i`, `a`, `o` | Type and edit text |
| **Visual** | `v`, `V`, `Ctrl+v` | Select text for operations |
| **Command** | `:` | Run ex-commands (save, quit, search/replace) |
| **Replace** | `R` | Overwrite existing text character by character |
| **Terminal** | `:terminal` | Embedded terminal emulator |

## Essential Keybindings

### Navigation

| Shortcut | Action |
|----------|--------|
| `h / j / k / l` | Move left / down / up / right |
| `w / b` | Jump forward / backward by word |
| `e` | Jump to end of word |
| `0 / $` | Jump to start / end of line |
| `gg / G` | Jump to top / bottom of file |
| `Ctrl+d / Ctrl+u` | Scroll half page down / up |
| `Ctrl+f / Ctrl+b` | Scroll full page down / up |
| `{ / }` | Jump to previous / next paragraph |
| `%` | Jump to matching bracket |
| `f{char} / F{char}` | Jump to next / previous occurrence of char |
| `H / M / L` | Move cursor to top / middle / bottom of screen |

### Editing

| Shortcut | Action |
|----------|--------|
| `i / a` | Insert before / after cursor |
| `I / A` | Insert at start / end of line |
| `o / O` | Open new line below / above |
| `x` | Delete character under cursor |
| `dd` | Delete entire line |
| `D` | Delete to end of line |
| `cc` | Change entire line |
| `C` | Change to end of line |
| `ciw` | Change inner word |
| `ci"` | Change inside quotes |
| `yy` | Yank (copy) line |
| `p / P` | Paste after / before cursor |
| `u / Ctrl+r` | Undo / redo |
| `.` | Repeat last change |
| `>>` / `<<` | Indent / outdent line |
| `J` | Join line below with current line |

### Search & Replace

| Shortcut | Action |
|----------|--------|
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n / N` | Next / previous match |
| `*` | Search for word under cursor |
| `:%s/old/new/g` | Replace all in file |
| `:%s/old/new/gc` | Replace all with confirmation |
| `:s/old/new/g` | Replace all on current line |

### Window Management

| Shortcut | Action |
|----------|--------|
| `:split` or `Ctrl+w s` | Horizontal split |
| `:vsplit` or `Ctrl+w v` | Vertical split |
| `Ctrl+w h/j/k/l` | Navigate between splits |
| `Ctrl+w =` | Equalize split sizes |
| `Ctrl+w _` | Maximize current split height |
| `Ctrl+w \|` | Maximize current split width |
| `Ctrl+w q` | Close current split |
| `Ctrl+w o` | Close all other splits |
| `Ctrl+w r` | Rotate splits |

### Buffers & Tabs

| Shortcut | Action |
|----------|--------|
| `:e filename` | Open file in new buffer |
| `:bn / :bp` | Next / previous buffer |
| `:bd` | Delete (close) buffer |
| `:ls` | List all buffers |
| `:tabnew` | Open new tab |
| `gt / gT` | Next / previous tab |
| `:tabclose` | Close current tab |

## Recommended Custom Keymaps

Add these to `lua/config/keymaps.lua`:

```lua
local map = vim.keymap.set

-- Set leader key to space
vim.g.mapleader = " "
vim.g.maplocalleader = " "

-- Better window navigation
map("n", "<C-h>", "<C-w>h", { desc = "Move to left split" })
map("n", "<C-j>", "<C-w>j", { desc = "Move to lower split" })
map("n", "<C-k>", "<C-w>k", { desc = "Move to upper split" })
map("n", "<C-l>", "<C-w>l", { desc = "Move to right split" })

-- Resize splits with arrow keys
map("n", "<C-Up>", ":resize +2<CR>", { desc = "Increase split height" })
map("n", "<C-Down>", ":resize -2<CR>", { desc = "Decrease split height" })
map("n", "<C-Left>", ":vertical resize -2<CR>", { desc = "Decrease split width" })
map("n", "<C-Right>", ":vertical resize +2<CR>", { desc = "Increase split width" })

-- Buffer navigation
map("n", "<S-l>", ":bnext<CR>", { desc = "Next buffer" })
map("n", "<S-h>", ":bprevious<CR>", { desc = "Previous buffer" })
map("n", "<leader>bd", ":bdelete<CR>", { desc = "Close buffer" })

-- Stay in indent mode
map("v", "<", "<gv", { desc = "Indent left and reselect" })
map("v", ">", ">gv", { desc = "Indent right and reselect" })

-- Move lines up/down
map("v", "J", ":m '>+1<CR>gv=gv", { desc = "Move selection down" })
map("v", "K", ":m '<-2<CR>gv=gv", { desc = "Move selection up" })

-- Keep cursor centered when scrolling
map("n", "<C-d>", "<C-d>zz", { desc = "Scroll down centered" })
map("n", "<C-u>", "<C-u>zz", { desc = "Scroll up centered" })

-- Keep cursor centered when searching
map("n", "n", "nzzzv", { desc = "Next match centered" })
map("n", "N", "Nzzzv", { desc = "Previous match centered" })

-- Paste without losing register
map("x", "<leader>p", '"_dP', { desc = "Paste without overwriting register" })

-- Quick save and quit
map("n", "<leader>w", ":w<CR>", { desc = "Save file" })
map("n", "<leader>q", ":q<CR>", { desc = "Quit" })
map("n", "<leader>x", ":x<CR>", { desc = "Save and quit" })

-- Clear search highlights
map("n", "<leader>h", ":nohlsearch<CR>", { desc = "Clear search highlights" })

-- Select all
map("n", "<C-a>", "ggVG", { desc = "Select all" })
```

## Plugin Manager: lazy.nvim

[lazy.nvim](https://github.com/folke/lazy.nvim) is the most popular modern plugin manager. Bootstrap it in `lua/config/lazy.lua`:

```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.fn.isdirectory(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup("plugins")
```

Then load it from your `init.lua`:

```lua
require("config.options")
require("config.keymaps")
require("config.lazy")
require("config.autocmds")
```

## Essential Plugins

### Telescope (Fuzzy Finder)

```lua
-- lua/plugins/telescope.lua
return {
  "nvim-telescope/telescope.nvim",
  dependencies = { "nvim-lua/plenary.nvim" },
  keys = {
    { "<leader>ff", "<cmd>Telescope find_files<cr>", desc = "Find files" },
    { "<leader>fg", "<cmd>Telescope live_grep<cr>", desc = "Grep in files" },
    { "<leader>fb", "<cmd>Telescope buffers<cr>", desc = "Find buffers" },
    { "<leader>fh", "<cmd>Telescope help_tags<cr>", desc = "Help tags" },
    { "<leader>fr", "<cmd>Telescope oldfiles<cr>", desc = "Recent files" },
  },
}
```

### Treesitter (Syntax Highlighting)

```lua
-- lua/plugins/treesitter.lua
return {
  "nvim-treesitter/nvim-treesitter",
  build = ":TSUpdate",
  config = function()
    require("nvim-treesitter.configs").setup({
      ensure_installed = {
        "lua", "python", "javascript", "typescript",
        "html", "css", "json", "bash", "markdown",
      },
      highlight = { enable = true },
      indent = { enable = true },
    })
  end,
}
```

### LSP (Language Server Protocol)

```lua
-- lua/plugins/lsp.lua
return {
  "neovim/nvim-lspconfig",
  dependencies = {
    "mason-org/mason.nvim",
    "mason-org/mason-lspconfig.nvim",
  },
  config = function()
    require("mason").setup()
    require("mason-lspconfig").setup({
      ensure_installed = { "lua_ls", "ts_ls", "pyright" },
    })

    local lspconfig = require("lspconfig")
    local on_attach = function(_, bufnr)
      local map = function(keys, func, desc)
        vim.keymap.set("n", keys, func, { buffer = bufnr, desc = desc })
      end
      map("gd", vim.lsp.buf.definition, "Go to definition")
      map("gr", vim.lsp.buf.references, "Go to references")
      map("K", vim.lsp.buf.hover, "Hover documentation")
      map("<leader>rn", vim.lsp.buf.rename, "Rename symbol")
      map("<leader>ca", vim.lsp.buf.code_action, "Code action")
    end

    lspconfig.lua_ls.setup({ on_attach = on_attach })
    lspconfig.ts_ls.setup({ on_attach = on_attach })
    lspconfig.pyright.setup({ on_attach = on_attach })
  end,
}
```

### File Explorer (neo-tree)

```lua
-- lua/plugins/neo-tree.lua
return {
  "nvim-neo-tree/neo-tree.nvim",
  dependencies = {
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons",
    "MunifTanjim/nui.nvim",
  },
  keys = {
    { "<leader>e", "<cmd>Neotree toggle<cr>", desc = "Toggle file explorer" },
  },
}
```

## Pre-built Configurations

If you want a fully configured Neovim setup out of the box, consider these popular distributions:

| Distribution | Description |
|-------------|-------------|
| **[LazyVim](https://www.lazyvim.org/)** | Opinionated, modern, well-maintained. Great starting point. |
| **[NvChad](https://nvchad.com/)** | Beautiful UI, fast, good defaults. |
| **[AstroNvim](https://astronvim.com/)** | Feature-rich, community-driven, easy to extend. |
| **[kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim)** | Minimal single-file config. Best for learning. |

> **Tip:** Start with **kickstart.nvim** if you want to understand how everything works, then graduate to a distribution like LazyVim once you're comfortable.

## Other Text Editors on Ubuntu

### VS Code

The most popular GUI editor for development.

```bash
# Install via snap
sudo snap install code --classic

# Or via apt
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
echo "deb [arch=amd64] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update && sudo apt install code
```

#### VS Code Settings

**Settings file:** `~/.config/Code/User/settings.json`

#### VS Code Keybindings

**Keybindings file:** `~/.config/Code/User/keybindings.json`

Each entry in `keybindings.json` is a JSON object with the following schema:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `key` | string | yes | Key combination (e.g., `ctrl+shift+p`, `ctrl+k ctrl+s`) |
| `command` | string | yes | Command ID to execute (prefix with `-` to remove a default binding) |
| `when` | string | no | Context condition (e.g., `editorTextFocus`, `!inDebugMode`) |
| `args` | any | no | Arguments passed to the command |

#### Common Command IDs

| Command ID | Default Key | Description |
|------------|-------------|-------------|
| `workbench.action.openSettings` | `Ctrl+,` | Open settings |
| `workbench.action.showCommands` | `Ctrl+Shift+P` | Open command palette |
| `workbench.action.quickOpen` | `Ctrl+P` | Quick open file |
| `workbench.action.toggleSidebar` | `Ctrl+B` | Toggle sidebar |
| `workbench.action.togglePanel` | `Ctrl+J` | Toggle bottom panel |
| `workbench.action.terminal.toggleTerminal` | `` Ctrl+` `` | Toggle integrated terminal |
| `workbench.action.splitEditor` | `Ctrl+\` | Split editor |
| `workbench.action.closeActiveEditor` | `Ctrl+W` | Close active editor |
| `workbench.action.closeAllEditors` | `Ctrl+K Ctrl+W` | Close all editors |
| `workbench.action.navigateBack` | `Alt+Left` | Go back |
| `workbench.action.navigateForward` | `Alt+Right` | Go forward |
| `editor.action.formatDocument` | `Ctrl+Shift+I` | Format document |
| `editor.action.commentLine` | `Ctrl+/` | Toggle line comment |
| `editor.action.blockComment` | `Ctrl+Shift+A` | Toggle block comment |
| `editor.action.rename` | `F2` | Rename symbol |
| `editor.action.goToDefinition` | `F12` | Go to definition |
| `editor.action.peekDefinition` | `Alt+F12` | Peek definition |
| `editor.action.goToReferences` | `Shift+F12` | Go to references |
| `editor.action.triggerSuggest` | `Ctrl+Space` | Trigger autocomplete |
| `editor.action.quickFix` | `Ctrl+.` | Quick fix |
| `workbench.action.files.save` | `Ctrl+S` | Save file |
| `workbench.action.files.saveAll` | `Ctrl+K S` | Save all files |

#### Example keybindings.json

```json
// ~/.config/Code/User/keybindings.json
[
  {
    "key": "ctrl+shift+t",
    "command": "workbench.action.terminal.toggleTerminal"
  },
  {
    "key": "ctrl+shift+e",
    "command": "workbench.action.toggleSidebarVisibility"
  },
  {
    "key": "ctrl+k ctrl+d",
    "command": "editor.action.formatDocument",
    "when": "editorTextFocus && !editorReadonly"
  },
  {
    "key": "ctrl+shift+/",
    "command": "editor.action.blockComment",
    "when": "editorTextFocus"
  },
  {
    "key": "alt+j",
    "command": "editor.action.moveLinesDownAction",
    "when": "editorTextFocus"
  },
  {
    "key": "alt+k",
    "command": "editor.action.moveLinesUpAction",
    "when": "editorTextFocus"
  },
  {
    "key": "ctrl+d",
    "command": "-editor.action.addSelectionToNextFindMatch"
  }
]
```

### Vim

The classic editor that Neovim is forked from. Still widely available and used.

```bash
sudo apt install vim
```

**Configuration file:** `~/.vimrc` (Vimscript only, no Lua support)

#### Example ~/.vimrc

```vim
" ~/.vimrc — Vim configuration
" Equivalent options to the Neovim config above, in Vimscript

" ── General ──────────────────────────────────────────────
set nocompatible              " Disable Vi compatibility
filetype plugin indent on     " Enable filetype detection, plugins, and indent
syntax on                     " Enable syntax highlighting

" ── Line Numbers ─────────────────────────────────────────
set number                    " Show line numbers
set relativenumber            " Relative line numbers

" ── Indentation ──────────────────────────────────────────
set tabstop=2                 " Tab width
set shiftwidth=2              " Indent width
set expandtab                 " Use spaces instead of tabs
set smartindent               " Auto-indent new lines
set autoindent                " Copy indent from current line

" ── Search ───────────────────────────────────────────────
set ignorecase                " Case-insensitive search
set smartcase                 " Case-sensitive if uppercase present
set nohlsearch                " Don't highlight all matches
set incsearch                 " Incremental search

" ── Appearance ───────────────────────────────────────────
set termguicolors             " True color support
set signcolumn=yes            " Always show sign column
set cursorline                " Highlight current line
set scrolloff=8               " Keep 8 lines above/below cursor
set nowrap                    " No line wrapping
set colorcolumn=80            " Show column marker at 80 chars
set showcmd                   " Show partial command in status line
set showmode                  " Show current mode
set laststatus=2              " Always show status line
set wildmenu                  " Enhanced command-line completion
set wildmode=longest:full,full

" ── Splits ───────────────────────────────────────────────
set splitbelow                " Horizontal splits open below
set splitright                " Vertical splits open right

" ── System ───────────────────────────────────────────────
set clipboard=unnamedplus     " Use system clipboard
set undofile                  " Persistent undo history
set undodir=~/.vim/undodir    " Undo file directory (create with mkdir -p)
set noswapfile                " No swap files
set nobackup                  " No backup files
set updatetime=250            " Faster CursorHold events
set ttimeoutlen=50            " Reduce key code delay
set mouse=a                   " Enable mouse support
set encoding=utf-8            " UTF-8 encoding
set backspace=indent,eol,start " Backspace over everything

" ── Key Mappings ─────────────────────────────────────────
let mapleader = " "

" Better window navigation
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

" Resize splits with arrow keys
nnoremap <C-Up> :resize +2<CR>
nnoremap <C-Down> :resize -2<CR>
nnoremap <C-Left> :vertical resize -2<CR>
nnoremap <C-Right> :vertical resize +2<CR>

" Buffer navigation
nnoremap <S-l> :bnext<CR>
nnoremap <S-h> :bprevious<CR>
nnoremap <leader>bd :bdelete<CR>

" Stay in indent mode
vnoremap < <gv
vnoremap > >gv

" Move lines up/down in visual mode
vnoremap J :m '>+1<CR>gv=gv
vnoremap K :m '<-2<CR>gv=gv

" Keep cursor centered when scrolling
nnoremap <C-d> <C-d>zz
nnoremap <C-u> <C-u>zz

" Quick save and quit
nnoremap <leader>w :w<CR>
nnoremap <leader>q :q<CR>
nnoremap <leader>x :x<CR>

" Clear search highlights
nnoremap <leader>h :nohlsearch<CR>
```

### nano

The simplest terminal editor — great for quick edits.

**Configuration file:** `~/.nanorc`

#### Available Options

| Option | Description |
|--------|-------------|
| `set linenumbers` | Show line numbers |
| `set tabsize N` | Set tab width to N spaces |
| `set tabstospaces` | Convert tabs to spaces |
| `set autoindent` | Auto-indent new lines to match previous line |
| `set mouse` | Enable mouse support |
| `set softwrap` | Wrap long lines visually (without inserting newlines) |
| `set atblanks` | When soft-wrapping, wrap at whitespace instead of mid-word |
| `set wordchars "chars"` | Characters considered part of a word |
| `set smooth` | Smooth scrolling (line by line instead of half-screen jumps) |
| `set boldtext` | Use bold instead of reverse video for highlighted text |
| `set brackets "chars"` | Characters treated as closing brackets for paragraph justify |
| `set casesensitive` | Make searches case-sensitive by default |
| `set constantshow` | Constantly show cursor position in status bar |
| `set cutfromcursor` | `Ctrl+K` cuts from cursor to end of line (not whole line) |
| `set fill N` | Hard-wrap lines at column N (0 = terminal width) |
| `set historylog` | Save search/replace history between sessions |
| `set indicator` | Show a scroll position indicator on the right |
| `set matchbrackets "pairs"` | Bracket pairs for matching (e.g., `"(<[{)>]}"`) |
| `set minibar` | Show a minimal bar with file info instead of title bar |
| `set nohelp` | Hide the shortcut help lines at the bottom |
| `set nonewlines` | Don't add a newline at end of file if missing |
| `set nowrap` | Don't hard-wrap lines (keeps long lines as-is) |
| `set numbercolor fgcolor` | Color for line numbers |
| `set positionlog` | Remember cursor position for previously opened files |
| `set quickblank` | Clear the status bar message after 1 keystroke |
| `set regexp` | Use regular expressions in search by default |
| `set showcursor` | Show cursor position in the title bar |
| `set smarthome` | Home key jumps to first non-whitespace character |
| `set speller "command"` | External spell checker command |
| `set stateflags` | Show state flags (modified, auto-indent, etc.) in title bar |
| `set suspendable` | Allow `Ctrl+Z` to suspend nano |
| `set trimblanks` | Remove trailing whitespace when saving |
| `set unix` | Save files in Unix format (LF line endings) |
| `set zap` | Delete/backspace erases the selected region |
| `include /path/to/file` | Include another nanorc file (e.g., syntax highlighting) |

#### Example ~/.nanorc

```bash
# ~/.nanorc — nano configuration

# Display
set linenumbers
set constantshow
set indicator
set minibar
set smooth
set showcursor
set stateflags

# Indentation
set tabsize 4
set tabstospaces
set autoindent
set smarthome
set trimblanks

# Editing behavior
set mouse
set zap
set cutfromcursor
set matchbrackets "(<[{)>]}"

# Wrapping
set softwrap
set atblanks
set nowrap

# History and state
set historylog
set positionlog

# Allow suspend with Ctrl+Z
set suspendable

# Include syntax highlighting definitions
include "/usr/share/nano/*.nanorc"
include "/usr/share/nano/extra/*.nanorc"
```

### Emacs

A powerful, extensible editor with its own ecosystem.

```bash
sudo apt install emacs
```

Configuration: `~/.emacs.d/init.el` or `~/.config/emacs/init.el`

## Best Practices

1. **Learn Vim motions first** — Before installing any plugins, spend time with `vimtutor` (run it in your terminal). The motions are the real power of Neovim.

2. **Use the leader key** — Map your most-used commands to `<leader>` + key combinations. Space is the most popular leader key.

3. **Don't copy entire configs** — Start minimal and add things as you need them. Understand each line of your config.

4. **Use LSP over ctags** — Modern language servers provide much better code intelligence than traditional ctags.

5. **Learn text objects** — Commands like `ciw` (change inner word), `da"` (delete around quotes), and `vi{` (select inside braces) are incredibly powerful.

6. **Leverage macros** — Record with `q{register}`, replay with `@{register}`. Use `@@` to repeat the last macro.

7. **Use marks for navigation** — Set marks with `m{letter}`, jump with `'{letter}`. Lowercase marks are buffer-local, uppercase are global.

8. **Master the dot command** — The `.` key repeats your last change. Structure your edits to be repeatable.

9. **Keep your config in version control** — Store your `~/.config/nvim/` in a git repo or dotfiles manager.

10. **Use `:checkhealth`** — Run `:checkhealth` in Neovim to diagnose configuration issues.

## Main Configuration Files in Ubuntu

Here is a reference of the most important configuration files on an Ubuntu system:

### Shell Configuration

| File | Purpose |
|------|---------|
| `~/.bashrc` | Bash shell configuration (aliases, prompt, functions) |
| `~/.zshrc` | Zsh shell configuration (if using Zsh) |
| `~/.profile` | Login shell environment variables |
| `~/.bash_aliases` | Dedicated file for bash aliases |
| `/etc/environment` | System-wide environment variables |
| `/etc/bash.bashrc` | System-wide bash configuration |

### Editor Configuration

| File | Purpose |
|------|---------|
| `~/.config/nvim/init.lua` | Neovim main config |
| `~/.vimrc` | Vim configuration |
| `~/.nanorc` | nano configuration |
| `~/.config/Code/User/settings.json` | VS Code settings |
| `~/.config/Code/User/keybindings.json` | VS Code keybindings |
| `~/.emacs.d/init.el` | Emacs configuration |

### Terminal & Appearance

| File | Purpose |
|------|---------|
| `~/.config/alacritty/alacritty.toml` | Alacritty terminal config |
| `~/.config/kitty/kitty.conf` | Kitty terminal config |
| `~/.config/wezterm/wezterm.lua` | WezTerm terminal config |
| `~/.config/gtk-3.0/settings.ini` | GTK3 theme settings |
| `~/.config/gtk-4.0/settings.ini` | GTK4 theme settings |

### Git & Development

| File | Purpose |
|------|---------|
| `~/.gitconfig` | Git global configuration |
| `~/.ssh/config` | SSH client configuration |
| `~/.npmrc` | Node.js/npm configuration |
| `~/.config/pip/pip.conf` | Python pip configuration |

### System Configuration

| File | Purpose |
|------|---------|
| `/etc/fstab` | Filesystem mount table |
| `/etc/hosts` | Hostname to IP mappings |
| `/etc/hostname` | System hostname |
| `/etc/apt/sources.list` | APT package repositories |
| `/etc/apt/sources.list.d/` | Additional APT repositories |
| `/etc/default/grub` | GRUB bootloader configuration |
| `/etc/sysctl.conf` | Kernel parameters |
| `/etc/sudoers` | Sudo access rules (edit with `visudo`) |
| `/etc/crontab` | System-wide scheduled tasks |
| `~/.config/autostart/` | Applications that start on login |

### GNOME / Desktop

| File | Purpose |
|------|---------|
| `~/.local/share/gnome-shell/extensions/` | GNOME Shell extensions |
| `~/.config/dconf/user` | GNOME/dconf settings database |
| `~/.local/share/applications/` | Custom .desktop application launchers |
| `~/.config/mimeapps.list` | Default application associations |

> **Tip:** Use `dconf dump / > full-backup.txt` to export all your GNOME/desktop settings, and `dconf load / < full-backup.txt` to restore them on a fresh install.
