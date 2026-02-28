---
title: Window Management Configuration
description: Learn how to configure and customize window management shortcuts in Ubuntu using GNOME settings, dconf, and gsettings.
category: window-management
order: 1
---

## Overview

Ubuntu's GNOME desktop provides powerful window management shortcuts out of the box. You can customize these through the Settings app, `gsettings` CLI, or `dconf-editor`.

## Using GNOME Settings

The easiest way to modify window shortcuts is through the GUI:

1. Open **Settings** → **Keyboard** → **Keyboard Shortcuts**
2. Scroll to the **Windows** section
3. Click any shortcut to reassign it
4. Press your desired key combination, or **Backspace** to disable

## Using gsettings CLI

For more control, use `gsettings` in the terminal:

```bash
# List all window keybindings
gsettings list-keys org.gnome.desktop.wm.keybindings

# View a specific binding
gsettings get org.gnome.desktop.wm.keybindings maximize

# Set a new binding (use array format)
gsettings set org.gnome.desktop.wm.keybindings maximize "['<Super>Up']"

# Disable a binding
gsettings set org.gnome.desktop.wm.keybindings minimize "['disabled']"
```

## Common Customizations

### Quarter Tiling

GNOME doesn't support quarter-tiling natively, but you can add it:

- Install the **Tiling Assistant** extension from GNOME Extensions
- Or use `xdotool` scripts bound to custom shortcuts

### Custom Shortcuts for Window Snapping

```bash
# Add a custom shortcut via gsettings
gsettings set org.gnome.settings-daemon.plugins.media-keys custom-keybindings \
  "['/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/']"

gsettings set org.gnome.settings-daemon.plugins.media-keys.custom-keybinding:/org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ \
  name "'Quarter tile top-left'"
```

## Resetting to Defaults

To restore all window management shortcuts to their defaults:

```bash
gsettings reset-recursively org.gnome.desktop.wm.keybindings
```

> **Tip:** Always back up your current settings before making changes with `dconf dump /org/gnome/desktop/wm/keybindings/ > wm-backup.txt`
