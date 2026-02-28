---
title: Tab Management Configuration
description: Configure tab shortcuts across browsers, terminal emulators, and file managers in Ubuntu.
category: tab-management
order: 3
---

## Overview

Tab management shortcuts are typically application-specific rather than system-wide. This guide covers customizing tab behavior in the most common Ubuntu applications.

## Browser Tab Shortcuts

Most browsers (Firefox, Chrome, Chromium) share the same tab shortcuts. These are generally not configurable through system settings but can be modified through browser extensions or config files.

### Firefox Custom Shortcuts

1. Install the **Shortkeys** or **Vimium** extension
2. Or modify `about:config` for some behaviors:
   - `browser.tabs.closeWindowWithLastTab` — prevent closing window when last tab closes
   - `browser.ctrlTab.sortByRecentlyUsed` — Ctrl+Tab cycles by recency instead of order

### Chrome Custom Shortcuts

Navigate to `chrome://extensions/shortcuts` to configure extension-specific keyboard shortcuts.

## GNOME Terminal

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

### Alternative Terminals

If you use a different terminal emulator, tab shortcuts are configured differently:

- **Tilix**: Settings → Keyboard → Shortcuts
- **Kitty**: Edit `~/.config/kitty/kitty.conf` and add `map` directives
- **Alacritty**: Edit `~/.config/alacritty/alacritty.toml` (uses tmux/screen for tabs)

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
