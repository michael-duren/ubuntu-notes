---
title: Window Management Configuration
description: Learn how to configure and customize window management shortcuts in Ubuntu using GNOME settings, dconf, and gsettings.
category: window-management
order: 1
---

## Overview

Ubuntu's GNOME desktop provides powerful window management shortcuts out of the box. You can customize these through the Settings app, `gsettings` CLI, or `dconf-editor`.

## Config File Paths

Window management keybindings are stored in the **dconf** database — a binary key-value store that backs `gsettings`.

| Path | Description |
|------|-------------|
| `~/.config/dconf/user` | Binary dconf database (all GNOME settings) |
| `/org/gnome/desktop/wm/keybindings/` | dconf path for window manager keybindings |
| `/org/gnome/mutter/keybindings/` | dconf path for Mutter compositor keybindings |
| `/org/gnome/mutter/` | dconf path for Mutter behavior settings (edge-tiling, etc.) |

You cannot edit the dconf database file directly — use `gsettings`, `dconf write`, or `dconf load` instead.

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

## Key Format Reference

Keybindings use a string format to express key combinations:

| Token | Meaning | Example |
|-------|---------|---------|
| `<Super>` | Super (Windows/Meta) key | `<Super>Up` |
| `<Ctrl>` | Control key | `<Ctrl>w` |
| `<Alt>` | Alt key | `<Alt>F4` |
| `<Shift>` | Shift key | `<Shift>Tab` |
| `<Super><Shift>` | Combined modifiers | `<Super><Shift>Left` |
| `disabled` | Unbind the key | `['disabled']` |
| Multiple bindings | Array with multiple entries | `['<Super>Up', '<Ctrl><Super>Up']` |

Key names use X11 keysym names — common ones: `Up`, `Down`, `Left`, `Right`, `Return`, `Tab`, `space`, `F1`–`F12`, `Home`, `End`, `Page_Up`, `Page_Down`, `BackSpace`, `Delete`.

## Schema Reference: org.gnome.desktop.wm.keybindings

These are the standard window manager keybindings. All values are string arrays (`as`).

### Window Actions

| Key | Default | Description |
|-----|---------|-------------|
| `close` | `['<Alt>F4']` | Close window |
| `maximize` | `['<Super>Up']` | Maximize window |
| `unmaximize` | `['<Super>Down']` | Restore/unmaximize window |
| `minimize` | `['<Super>h']` | Minimize window |
| `toggle-fullscreen` | `[]` (disabled) | Toggle fullscreen mode |
| `toggle-maximized` | `['<Super>Up']` | Toggle maximized state |
| `always-on-top` | `[]` (disabled) | Toggle always-on-top |
| `toggle-above` | `[]` (disabled) | Toggle keep above other windows |
| `toggle-on-all-workspaces` | `[]` (disabled) | Show window on all workspaces |
| `raise` | `[]` (disabled) | Raise window above others |
| `lower` | `[]` (disabled) | Lower window below others |

### Window Movement & Resizing

| Key | Default | Description |
|-----|---------|-------------|
| `begin-move` | `['<Alt>F7']` | Start keyboard-driven window move |
| `begin-resize` | `['<Alt>F8']` | Start keyboard-driven window resize |
| `move-to-center` | `[]` (disabled) | Move window to center of screen |
| `move-to-corner-ne` | `[]` (disabled) | Move to top-right corner |
| `move-to-corner-nw` | `[]` (disabled) | Move to top-left corner |
| `move-to-corner-se` | `[]` (disabled) | Move to bottom-right corner |
| `move-to-corner-sw` | `[]` (disabled) | Move to bottom-left corner |
| `move-to-side-n` | `[]` (disabled) | Move to top edge |
| `move-to-side-s` | `[]` (disabled) | Move to bottom edge |
| `move-to-side-e` | `[]` (disabled) | Move to right edge |
| `move-to-side-w` | `[]` (disabled) | Move to left edge |

### Monitor Management

| Key | Default | Description |
|-----|---------|-------------|
| `move-to-monitor-left` | `['<Super><Shift>Left']` | Move window to left monitor |
| `move-to-monitor-right` | `['<Super><Shift>Right']` | Move window to right monitor |
| `move-to-monitor-up` | `['<Super><Shift>Up']` | Move window to upper monitor |
| `move-to-monitor-down` | `['<Super><Shift>Down']` | Move window to lower monitor |

### Workspace Navigation

| Key | Default | Description |
|-----|---------|-------------|
| `switch-to-workspace-1` | `[]` (disabled) | Switch to workspace 1 |
| `switch-to-workspace-2` | `[]` (disabled) | Switch to workspace 2 |
| `switch-to-workspace-3` | `[]` (disabled) | Switch to workspace 3 |
| `switch-to-workspace-4` | `[]` (disabled) | Switch to workspace 4 |
| `switch-to-workspace-up` | `['<Super>Page_Up']` | Switch to workspace above |
| `switch-to-workspace-down` | `['<Super>Page_Down']` | Switch to workspace below |
| `switch-to-workspace-left` | `[]` (disabled) | Switch to workspace left |
| `switch-to-workspace-right` | `[]` (disabled) | Switch to workspace right |
| `switch-to-workspace-last` | `[]` (disabled) | Switch to last workspace |

### Move Window to Workspace

| Key | Default | Description |
|-----|---------|-------------|
| `move-to-workspace-1` | `[]` (disabled) | Move window to workspace 1 |
| `move-to-workspace-2` | `[]` (disabled) | Move window to workspace 2 |
| `move-to-workspace-3` | `[]` (disabled) | Move window to workspace 3 |
| `move-to-workspace-4` | `[]` (disabled) | Move window to workspace 4 |
| `move-to-workspace-up` | `['<Super><Shift>Page_Up']` | Move window to workspace above |
| `move-to-workspace-down` | `['<Super><Shift>Page_Down']` | Move window to workspace below |
| `move-to-workspace-left` | `[]` (disabled) | Move window to workspace left |
| `move-to-workspace-right` | `[]` (disabled) | Move window to workspace right |
| `move-to-workspace-last` | `[]` (disabled) | Move window to last workspace |

### Window Cycling & Switching

| Key | Default | Description |
|-----|---------|-------------|
| `switch-windows` | `['<Alt>Tab']` | Switch between windows |
| `switch-windows-backward` | `['<Shift><Alt>Tab']` | Switch windows in reverse |
| `switch-applications` | `['<Super>Tab']` | Switch between applications |
| `switch-applications-backward` | `['<Shift><Super>Tab']` | Switch applications in reverse |
| `switch-group` | `['<Super>Above_Tab']` | Switch between windows of same app |
| `switch-group-backward` | `['<Shift><Super>Above_Tab']` | Switch same-app windows in reverse |
| `cycle-windows` | `[]` (disabled) | Cycle through all windows |
| `cycle-windows-backward` | `[]` (disabled) | Cycle windows in reverse |
| `cycle-group` | `[]` (disabled) | Cycle windows of same app |
| `cycle-group-backward` | `[]` (disabled) | Cycle same-app in reverse |
| `switch-panels` | `['<Ctrl><Alt>Tab']` | Switch between panels |
| `switch-panels-backward` | `['<Shift><Ctrl><Alt>Tab']` | Switch panels in reverse |

### Other

| Key | Default | Description |
|-----|---------|-------------|
| `panel-run-dialog` | `['<Alt>F2']` | Open run dialog |
| `activate-window-menu` | `['<Alt>space']` | Open window menu |
| `show-desktop` | `[]` (disabled) | Show/hide desktop |

## Schema Reference: org.gnome.mutter.keybindings

These are the Mutter compositor-level keybindings. All values are string arrays (`as`).

| Key | Default | Description |
|-----|---------|-------------|
| `toggle-tiled-left` | `[]` (disabled) | Tile window to left half |
| `toggle-tiled-right` | `[]` (disabled) | Tile window to right half |
| `switch-monitor` | `['<Super>p', 'XF86Display']` | Switch/configure display mode |
| `rotate-monitor` | `['XF86RotateWindows']` | Rotate monitor orientation |
| `cancel-input-capture` | `['<Super>Escape']` | Cancel input capture |

> **Note:** On Ubuntu, `<Super>Left` and `<Super>Right` tile windows via GNOME Shell's built-in tiling, which uses the mutter edge-tiling feature rather than these keybindings.

## Example dconf Dump

Use `dconf dump` to export your window management settings as a portable text file. Here's an example of a customized configuration:

```ini
# Export: dconf dump /org/gnome/desktop/wm/keybindings/ > wm-keybindings.ini
# Import: dconf load /org/gnome/desktop/wm/keybindings/ < wm-keybindings.ini

[/]
close=['<Super>q']
maximize=['<Super>Up']
unmaximize=['<Super>Down']
minimize=['<Super>comma']
toggle-fullscreen=['<Super>f']
begin-move=['<Super>F7']
begin-resize=['<Super>F8']
move-to-monitor-left=['<Super><Shift>Left']
move-to-monitor-right=['<Super><Shift>Right']
switch-to-workspace-1=['<Super>1']
switch-to-workspace-2=['<Super>2']
switch-to-workspace-3=['<Super>3']
switch-to-workspace-4=['<Super>4']
move-to-workspace-1=['<Super><Shift>1']
move-to-workspace-2=['<Super><Shift>2']
move-to-workspace-3=['<Super><Shift>3']
move-to-workspace-4=['<Super><Shift>4']
switch-windows=['<Alt>Tab']
switch-applications=['<Super>Tab']
panel-run-dialog=['<Alt>F2']
show-desktop=['<Super>d']
```

```ini
# Export: dconf dump /org/gnome/mutter/keybindings/ > mutter-keybindings.ini
# Import: dconf load /org/gnome/mutter/keybindings/ < mutter-keybindings.ini

[/]
toggle-tiled-left=['<Super>Left']
toggle-tiled-right=['<Super>Right']
switch-monitor=['<Super>p', 'XF86Display']
```

## Tiling Assistant Extension

GNOME's built-in tiling only supports half-screen left/right snapping. For quarter tiling (1/4), thirds (1/3, 2/3), and other advanced layouts, install the **Tiling Assistant** extension.

### Installing Tiling Assistant

```bash
# 1. Install the GNOME Extensions browser connector (if you haven't already)
sudo apt install gnome-shell-extension-manager

# 2. Open Extension Manager and search for "Tiling Assistant"
#    Or install from the command line using gnome-extensions-cli:
pip install gnome-extensions-cli
gext install tiling-assistant@leleat-on-github
```

You can also install it from the browser at **extensions.gnome.org** — search for "Tiling Assistant" by Leleat.

After installing, log out and back in (or press `Alt+F2`, type `r`, press Enter on X11) to activate it.

### How Tiling Assistant Works

Tiling Assistant extends GNOME's built-in edge-tiling. It uses the dconf schema `org.gnome.shell.extensions.tiling-assistant` for all its settings and keybindings.

**dconf path:** `/org/gnome/shell/extensions/tiling-assistant/`

### Configuring Shortcuts via CLI

All Tiling Assistant keybindings live under the `org.gnome.shell.extensions.tiling-assistant` schema. You can set them with `gsettings` or `dconf write`:

```bash
# ── Half Tiling ──────────────────────────────────────────
# These override GNOME's built-in Super+Left/Right

gsettings set org.gnome.shell.extensions.tiling-assistant tile-left-half "['<Super>Left']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-right-half "['<Super>Right']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-top-half "['<Super>Up']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-bottom-half "['<Super>Down']"

# ── Quarter Tiling ───────────────────────────────────────

gsettings set org.gnome.shell.extensions.tiling-assistant tile-topleft-quarter "['<Super>u']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-topright-quarter "['<Super>i']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-bottomleft-quarter "['<Super>j']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-bottomright-quarter "['<Super>k']"

# ── Third Tiling ─────────────────────────────────────────
# Useful for ultrawide monitors

gsettings set org.gnome.shell.extensions.tiling-assistant tile-left-half-ignore-ta "['<Super><Ctrl>Left']"
gsettings set org.gnome.shell.extensions.tiling-assistant tile-right-half-ignore-ta "['<Super><Ctrl>Right']"

# ── Maximize / Center ────────────────────────────────────

gsettings set org.gnome.shell.extensions.tiling-assistant tile-maximize "['<Super>Up']"
gsettings set org.gnome.shell.extensions.tiling-assistant center-window "['<Super>c']"
```

### Keybinding Reference

| Key | Suggested Shortcut | Description |
|-----|-------------------|-------------|
| `tile-left-half` | `<Super>Left` | Tile to left 1/2 |
| `tile-right-half` | `<Super>Right` | Tile to right 1/2 |
| `tile-top-half` | `<Super>Up` | Tile to top 1/2 |
| `tile-bottom-half` | `<Super>Down` | Tile to bottom 1/2 |
| `tile-topleft-quarter` | `<Super>u` | Tile to top-left 1/4 |
| `tile-topright-quarter` | `<Super>i` | Tile to top-right 1/4 |
| `tile-bottomleft-quarter` | `<Super>j` | Tile to bottom-left 1/4 |
| `tile-bottomright-quarter` | `<Super>k` | Tile to bottom-right 1/4 |
| `tile-maximize` | `<Super>Up` | Maximize/fill available space |
| `center-window` | `<Super>c` | Center window on screen |
| `tile-left-half-ignore-ta` | `<Super><Ctrl>Left` | Left half (ignore Tiling Assistant layout) |
| `tile-right-half-ignore-ta` | `<Super><Ctrl>Right` | Right half (ignore Tiling Assistant layout) |
| `restore-window` | `<Super>Down` | Restore tiled window to floating |
| `toggle-always-on-top` | — | Toggle always-on-top |
| `toggle-tiling-popup` | — | Toggle the tiling popup for remaining space |

### Behavior Settings

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `enable-tiling-popup` | `b` | `true` | After tiling one window, show popup to tile another in the remaining space |
| `tiling-popup-all-workspace` | `b` | `false` | Include windows from all workspaces in tiling popup |
| `enable-raise-tile-group` | `b` | `true` | Raise all tiled windows in a group together |
| `tilegroups-in-app-switcher` | `b` | `false` | Show tile groups as single entries in Alt+Tab |
| `maximize-with-gap` | `b` | `false` | Keep gaps when a window is maximized |
| `window-gap` | `i` | `0` | Gap between tiled windows (pixels) |
| `single-screen-gap` | `i` | `0` | Gap between tiled windows and screen edges (pixels) |
| `screen-top-gap` | `i` | `-1` | Override top gap (-1 = use `single-screen-gap`) |
| `screen-bottom-gap` | `i` | `-1` | Override bottom gap |
| `screen-left-gap` | `i` | `-1` | Override left gap |
| `screen-right-gap` | `i` | `-1` | Override right gap |
| `dynamic-keybinding-behavior` | `i` | `0` | What happens when you press a tiling shortcut twice: `0` = focus, `1` = cycle through sizes (1/2 → 1/3 → 2/3) |

### Enabling 1/3 and 2/3 Tiling

The key setting for thirds support is `dynamic-keybinding-behavior`. When set to **cycle** mode, pressing the same tiling shortcut repeatedly cycles through sizes:

```bash
# Enable cycling: pressing Super+Left multiple times cycles 1/2 → 1/3 → 2/3
gsettings set org.gnome.shell.extensions.tiling-assistant dynamic-keybinding-behavior 1
```

With this enabled:
- Press `Super+Left` once → left 1/2
- Press `Super+Left` again → left 1/3
- Press `Super+Left` again → left 2/3
- Press `Super+Left` again → back to left 1/2

This works for all half-tiling shortcuts (left, right, top, bottom).

### Adding Gaps Between Windows

```bash
# Set 8px gap between tiled windows
gsettings set org.gnome.shell.extensions.tiling-assistant window-gap 8

# Set 8px gap between windows and screen edges
gsettings set org.gnome.shell.extensions.tiling-assistant single-screen-gap 8
```

### Example dconf Dump

```ini
# Export: dconf dump /org/gnome/shell/extensions/tiling-assistant/ > tiling-assistant.ini
# Import: dconf load /org/gnome/shell/extensions/tiling-assistant/ < tiling-assistant.ini

[/]
tile-left-half=['<Super>Left']
tile-right-half=['<Super>Right']
tile-top-half=['<Super>Up']
tile-bottom-half=['<Super>Down']
tile-topleft-quarter=['<Super>u']
tile-topright-quarter=['<Super>i']
tile-bottomleft-quarter=['<Super>j']
tile-bottomright-quarter=['<Super>k']
tile-maximize=['<Super>m']
center-window=['<Super>c']
restore-window=['<Super>Down']
dynamic-keybinding-behavior=1
enable-tiling-popup=true
tiling-popup-all-workspace=false
enable-raise-tile-group=true
window-gap=8
single-screen-gap=8
maximize-with-gap=false
```

### Configuring via GUI

You can also configure everything through the extension's preferences UI:

1. Open **Extension Manager** (or GNOME Extensions app)
2. Find **Tiling Assistant** and click the gear icon
3. The **Keybindings** tab lets you set all shortcuts visually
4. The **General** tab controls gaps, popups, and cycling behavior

## Common Customizations

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
gsettings reset-recursively org.gnome.mutter.keybindings
```

> **Tip:** Always back up your current settings before making changes with `dconf dump /org/gnome/desktop/wm/keybindings/ > wm-backup.txt`
