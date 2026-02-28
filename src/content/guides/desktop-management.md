---
title: Desktop & Workspace Configuration
description: Configure workspace behavior, hot corners, and desktop navigation shortcuts in Ubuntu GNOME.
category: desktop-management
order: 2
---

## Overview

Ubuntu uses dynamic workspaces by default — new workspaces are created as needed and removed when empty. You can customize workspace behavior, navigation shortcuts, and more.

## Workspace Settings

### Dynamic vs Static Workspaces

```bash
# Check current mode
gsettings get org.gnome.mutter dynamic-workspaces

# Switch to static workspaces
gsettings set org.gnome.mutter dynamic-workspaces false

# Set number of static workspaces
gsettings set org.gnome.desktop.wm.preferences num-workspaces 4
```

### Workspaces on All Monitors

By default, workspaces only apply to the primary monitor:

```bash
# Make workspaces span all monitors
gsettings set org.gnome.mutter workspaces-only-on-primary false
```

## Hot Corners

The top-left hot corner triggers the Activities overview:

```bash
# Disable hot corner
gsettings set org.gnome.desktop.interface enable-hot-corners false

# Re-enable
gsettings set org.gnome.desktop.interface enable-hot-corners true
```

## Custom Navigation Shortcuts

### Adding Workspace Shortcuts

You can bind direct workspace access (jump to workspace 1, 2, 3, etc.):

```bash
# Jump directly to workspace 1
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-1 "['<Super>1']"

# Jump directly to workspace 2
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-2 "['<Super>2']"

# Move window to workspace 1
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-1 "['<Super><Shift>1']"
```

## Recommended Extensions

- **Workspace Indicator** — Shows current workspace number in the top bar
- **Custom Hot Corners** — Configure actions for all four screen corners
- **Dash to Panel** — Combines the dash and top panel for a more traditional desktop layout

## Resetting Workspace Settings

```bash
gsettings reset org.gnome.mutter dynamic-workspaces
gsettings reset org.gnome.desktop.wm.preferences num-workspaces
gsettings reset-recursively org.gnome.desktop.wm.keybindings
```

> **Tip:** Use `dconf watch /` in a terminal to see real-time changes as you modify settings through the GUI.
