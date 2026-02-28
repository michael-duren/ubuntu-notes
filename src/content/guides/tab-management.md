---
title: Tab Management Configuration
description: Configure tab shortcuts across browsers, terminal emulators, and file managers in Ubuntu.
category: tab-management
order: 3
---

## Overview

Tab management shortcuts are typically application-specific rather than system-wide. This guide covers customizing tab behavior in the most common Ubuntu applications.

## Browser Tab Shortcuts

> **Important:** On Linux, Firefox and Chrome use **different** shortcuts for switching to a specific tab by number.

### Default Tab Shortcuts

| Action | Firefox (Linux) | Chrome/Chromium (Linux) |
|---|---|---|
| Switch to tab 1–8 | `Alt + 1–8` | `Ctrl + 1–8` |
| Switch to last tab | `Alt + 9` | `Ctrl + 9` |
| New tab | `Ctrl + T` | `Ctrl + T` |
| Close tab | `Ctrl + W` | `Ctrl + W` |
| Next tab | `Ctrl + Tab` | `Ctrl + Tab` |
| Previous tab | `Ctrl + Shift + Tab` | `Ctrl + Shift + Tab` |
| Reopen closed tab | `Ctrl + Shift + T` | `Ctrl + Shift + T` |

### Firefox Custom Shortcuts

1. Install the **Shortkeys** or **Vimium** extension
2. Or modify `about:config` for some behaviors:
   - `browser.tabs.closeWindowWithLastTab` — prevent closing window when last tab closes
   - `browser.ctrlTab.sortByRecentlyUsed` — Ctrl+Tab cycles by recency instead of order

### Firefox Config Files

Firefox profiles live in `~/.mozilla/firefox/<profile-name>/`. You can create a `user.js` file in your profile directory to set preferences that apply on every startup:

**Path:** `~/.mozilla/firefox/<profile-name>/user.js`

#### Useful `about:config` Tab Preferences

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `browser.tabs.closeWindowWithLastTab` | `boolean` | `true` | Close window when last tab is closed |
| `browser.ctrlTab.sortByRecentlyUsed` | `boolean` | `false` | Ctrl+Tab cycles by recent use instead of order |
| `browser.tabs.warnOnClose` | `boolean` | `true` | Warn before closing multiple tabs |
| `browser.tabs.warnOnOpen` | `boolean` | `true` | Warn before opening many tabs at once |
| `browser.tabs.maxOpenBeforeWarn` | `integer` | `15` | Tab count threshold for open warning |
| `browser.tabs.loadInBackground` | `boolean` | `true` | Open new tabs in background |
| `browser.tabs.insertAfterCurrent` | `boolean` | `false` | Open new tabs next to current (instead of at end) |
| `browser.tabs.insertRelatedAfterCurrent` | `boolean` | `true` | Links open next to current tab |
| `browser.tabs.loadBookmarksInTabs` | `boolean` | `false` | Open bookmarks in new tab instead of current |
| `browser.tabs.tabMinWidth` | `integer` | `76` | Minimum tab width in pixels |
| `browser.search.openintab` | `boolean` | `false` | Open search results in a new tab |
| `browser.urlbar.openintab` | `boolean` | `false` | Open URL bar results in a new tab |
| `browser.tabs.loadDivertedInBackground` | `boolean` | `false` | Keep focus when external app opens a tab |

#### Example `user.js`

```javascript
// ~/.mozilla/firefox/<profile-name>/user.js
// These preferences are applied every time Firefox starts.
// To find your profile path: about:profiles

// Tab behavior
user_pref("browser.tabs.closeWindowWithLastTab", false);
user_pref("browser.ctrlTab.sortByRecentlyUsed", true);
user_pref("browser.tabs.insertAfterCurrent", true);
user_pref("browser.tabs.loadInBackground", true);
user_pref("browser.tabs.warnOnClose", false);

// Open searches and bookmarks in new tabs
user_pref("browser.search.openintab", true);
user_pref("browser.urlbar.openintab", true);
user_pref("browser.tabs.loadBookmarksInTabs", true);

// Tab appearance
user_pref("browser.tabs.tabMinWidth", 100);
```

### Chrome Custom Shortcuts

Navigate to `chrome://extensions/shortcuts` to configure extension-specific keyboard shortcuts.

## GNOME Terminal

### Default Terminal Tab Shortcuts

| Action | Shortcut |
|---|---|
| New tab | `Ctrl + Shift + T` |
| Close tab | `Ctrl + Shift + W` |
| Next tab | `Ctrl + Page Down` |
| Previous tab | `Ctrl + Page Up` |
| Switch to tab 1–9 | `Alt + 1–9` |
| Switch to tab 10 | `Alt + 0` |

### Customizing Terminal Tab Shortcuts

```bash
# List current terminal keybindings
gsettings list-keys org.gnome.Terminal.Legacy.Keybindings

# Change new tab shortcut
dconf write /org/gnome/terminal/legacy/keybindings/new-tab "'<Ctrl><Alt>t'"

# Change close tab shortcut
dconf write /org/gnome/terminal/legacy/keybindings/close-tab "'<Ctrl><Shift>q'"

# Change next/prev tab
dconf write /org/gnome/terminal/legacy/keybindings/next-tab "'<Ctrl>Right'"
dconf write /org/gnome/terminal/legacy/keybindings/prev-tab "'<Ctrl>Left'"
```

### Schema Reference: org.gnome.Terminal.Legacy.Keybindings

**dconf path:** `/org/gnome/terminal/legacy/keybindings/`

All values are strings. Use `'disabled'` to unbind a key.

| Key | Default | Description |
|-----|---------|-------------|
| `new-tab` | `'<Ctrl><Shift>t'` | Open new tab |
| `close-tab` | `'<Ctrl><Shift>w'` | Close current tab |
| `new-window` | `'<Ctrl><Shift>n'` | Open new window |
| `close-window` | `'<Ctrl><Shift>q'` | Close window |
| `next-tab` | `'<Ctrl>Page_Down'` | Switch to next tab |
| `prev-tab` | `'<Ctrl>Page_Up'` | Switch to previous tab |
| `move-tab-left` | `'<Ctrl><Shift>Page_Up'` | Move tab left |
| `move-tab-right` | `'<Ctrl><Shift>Page_Down'` | Move tab right |
| `switch-to-tab-1` | `'disabled'` | Switch to tab 1 |
| `switch-to-tab-2` | `'disabled'` | Switch to tab 2 |
| `switch-to-tab-3` | `'disabled'` | Switch to tab 3 |
| `switch-to-tab-4` | `'disabled'` | Switch to tab 4 |
| `switch-to-tab-5` | `'disabled'` | Switch to tab 5 |
| `switch-to-tab-6` | `'disabled'` | Switch to tab 6 |
| `switch-to-tab-7` | `'disabled'` | Switch to tab 7 |
| `switch-to-tab-8` | `'disabled'` | Switch to tab 8 |
| `switch-to-tab-9` | `'disabled'` | Switch to tab 9 |
| `switch-to-tab-10` | `'disabled'` | Switch to tab 10 |
| `copy` | `'<Ctrl><Shift>c'` | Copy selection |
| `paste` | `'<Ctrl><Shift>v'` | Paste clipboard |
| `select-all` | `'disabled'` | Select all text |
| `find` | `'<Ctrl><Shift>f'` | Open find bar |
| `find-next` | `'<Ctrl><Shift>g'` | Find next match |
| `find-previous` | `'<Ctrl><Shift>h'` | Find previous match |
| `find-clear` | `'<Ctrl><Shift>j'` | Clear find highlights |
| `zoom-in` | `'<Ctrl>plus'` | Increase font size |
| `zoom-out` | `'<Ctrl>minus'` | Decrease font size |
| `zoom-normal` | `'<Ctrl>0'` | Reset font size |
| `full-screen` | `'F11'` | Toggle fullscreen |
| `toggle-menubar` | `'disabled'` | Toggle menu bar visibility |
| `reset` | `'disabled'` | Reset terminal |
| `reset-and-clear` | `'disabled'` | Reset and clear terminal |
| `help` | `'disabled'` | Open help |
| `preferences` | `'disabled'` | Open preferences |

#### Example dconf Dump

```ini
# Export: dconf dump /org/gnome/terminal/legacy/keybindings/ > terminal-keys.ini
# Import: dconf load /org/gnome/terminal/legacy/keybindings/ < terminal-keys.ini

[/]
new-tab='<Ctrl><Shift>t'
close-tab='<Ctrl><Shift>w'
next-tab='<Ctrl>Right'
prev-tab='<Ctrl>Left'
move-tab-left='<Ctrl><Shift>Left'
move-tab-right='<Ctrl><Shift>Right'
switch-to-tab-1='<Alt>1'
switch-to-tab-2='<Alt>2'
switch-to-tab-3='<Alt>3'
switch-to-tab-4='<Alt>4'
copy='<Ctrl><Shift>c'
paste='<Ctrl><Shift>v'
find='<Ctrl><Shift>f'
zoom-in='<Ctrl>plus'
zoom-out='<Ctrl>minus'
zoom-normal='<Ctrl>0'
full-screen='F11'
```

## Kitty Terminal

**Config file:** `~/.config/kitty/kitty.conf`

Kitty has built-in tab support. Configure tab keybindings with the `map` directive.

### Tab-Related Options

| Option | Default | Description |
|--------|---------|-------------|
| `tab_bar_edge` | `bottom` | Tab bar position: `top` or `bottom` |
| `tab_bar_style` | `fade` | Style: `fade`, `separator`, `powerline`, `slant`, `hidden` |
| `tab_bar_min_tabs` | `2` | Minimum tabs before showing tab bar |
| `tab_separator` | `" ┇ "` | Separator between tabs (for `separator` style) |
| `tab_title_template` | `"{title}"` | Template for tab titles (supports `{index}`, `{title}`, `{num_windows}`) |
| `active_tab_title_template` | `none` | Template for the active tab (falls back to `tab_title_template`) |
| `tab_bar_background` | `none` | Tab bar background color |
| `active_tab_foreground` | `#000` | Active tab text color |
| `active_tab_background` | `#eee` | Active tab background color |
| `active_tab_font_style` | `bold-italic` | Active tab font style |
| `inactive_tab_foreground` | `#444` | Inactive tab text color |
| `inactive_tab_background` | `#999` | Inactive tab background color |
| `inactive_tab_font_style` | `normal` | Inactive tab font style |
| `tab_activity_symbol` | `none` | Symbol shown for tabs with activity |

### Default Tab Keybindings

| Action | Default Binding |
|--------|----------------|
| New tab | `Ctrl + Shift + T` |
| Close tab | `Ctrl + Shift + Q` |
| Next tab | `Ctrl + Shift + Right` |
| Previous tab | `Ctrl + Shift + Left` |
| Move tab forward | `Ctrl + Shift + .` |
| Move tab backward | `Ctrl + Shift + ,` |
| Set tab title | `Ctrl + Shift + Alt + T` |
| Go to tab N | `Ctrl + Shift + 1–9` (via kitty_mod) |

### Example Config Snippet

```bash
# ~/.config/kitty/kitty.conf — Tab configuration

# Tab bar appearance
tab_bar_edge            top
tab_bar_style           powerline
tab_bar_min_tabs        1
tab_title_template      "{index}: {title}"
active_tab_font_style   bold

# Tab colors
active_tab_foreground   #1e1e2e
active_tab_background   #cdd6f4
inactive_tab_foreground #6c7086
inactive_tab_background #313244

# Tab keybindings
map ctrl+shift+t        new_tab_with_cwd
map ctrl+shift+q        close_tab
map ctrl+shift+right    next_tab
map ctrl+shift+left     previous_tab
map ctrl+shift+.        move_tab_forward
map ctrl+shift+,        move_tab_backward
map ctrl+1              goto_tab 1
map ctrl+2              goto_tab 2
map ctrl+3              goto_tab 3
map ctrl+4              goto_tab 4
map ctrl+5              goto_tab 5
map ctrl+shift+alt+t    set_tab_title
```

## Alacritty Terminal

**Config file:** `~/.config/alacritty/alacritty.toml`

Alacritty is a minimal terminal — it has no built-in tabs or splits. Use it with a terminal multiplexer like **tmux** or **screen**, or use its keybinding system to launch new windows.

### Keybinding Schema

Alacritty keybindings are defined in TOML under `[[keyboard.bindings]]`:

| Field | Type | Description |
|-------|------|-------------|
| `key` | string | Key name (`T`, `W`, `Tab`, `Return`, `1`–`9`, etc.) |
| `mods` | string | Modifiers: `Control`, `Shift`, `Alt`, `Super` (combine with `\|`) |
| `action` | string | Built-in action (`CreateNewWindow`, `Quit`, `Copy`, `Paste`, `ScrollPageUp`, etc.) |
| `command` | string/object | External command to run instead of a built-in action |
| `mode` | string | Vi mode filter: `~Vi` (not in Vi mode), `Vi` (in Vi mode) |

### Example Config Snippet

```toml
# ~/.config/alacritty/alacritty.toml

[font]
size = 12.0

[window]
padding = { x = 4, y = 4 }
dynamic_padding = true

# Keybindings — since Alacritty has no tabs, bind keys
# to work with tmux or open new windows
[[keyboard.bindings]]
key = "T"
mods = "Control|Shift"
action = "CreateNewWindow"

[[keyboard.bindings]]
key = "N"
mods = "Control|Shift"
action = "CreateNewWindow"

[[keyboard.bindings]]
key = "Return"
mods = "Control|Shift"
action = "CreateNewWindow"

[[keyboard.bindings]]
key = "C"
mods = "Control|Shift"
action = "Copy"

[[keyboard.bindings]]
key = "V"
mods = "Control|Shift"
action = "Paste"

[[keyboard.bindings]]
key = "Equals"
mods = "Control"
action = "IncreaseFontSize"

[[keyboard.bindings]]
key = "Minus"
mods = "Control"
action = "DecreaseFontSize"

[[keyboard.bindings]]
key = "Key0"
mods = "Control"
action = "ResetFontSize"

# Example: bind a key to send tmux prefix + command
# This sends Ctrl+B then C to create a new tmux window
[[keyboard.bindings]]
key = "T"
mods = "Super"
command = { program = "tmux", args = ["new-window"] }
```

> **Tip:** For a tab-like experience in Alacritty, pair it with **tmux**. Add `command = { program = "tmux", args = ["new-window"] }` to your keybindings to create tmux windows with a keyboard shortcut.

## Nautilus (Files)

Nautilus tab shortcuts follow the standard `Ctrl+T` / `Ctrl+W` pattern. To customize:

```bash
# Nautilus uses standard GTK keybindings
# You can modify via the gtk-keys.conf or use custom CSS
```

## Tips for Power Users

- Use `xdotool` to create system-wide tab switching shortcuts that work across all apps
- Consider using `wmctrl` combined with custom shortcuts for advanced window+tab management
- Tools like `tmux` or `screen` provide tab-like functionality in any terminal

> **Tip:** If your keyboard has extra buttons (mouse buttons, media keys), you can bind those to tab navigation using `xbindkeys`.
