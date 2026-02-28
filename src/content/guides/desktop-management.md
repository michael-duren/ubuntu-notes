---
title: Desktop & Workspace Configuration
description: Configure workspace behavior, hot corners, and desktop navigation shortcuts in Ubuntu GNOME.
category: desktop-management
order: 2
---

## Overview

Ubuntu uses dynamic workspaces by default — new workspaces are created as needed and removed when empty. You can customize workspace behavior, navigation shortcuts, and more.

## Config File Paths

Desktop and workspace settings are stored in the **dconf** database across several schema paths:

| dconf Path | Schema | Description |
|------------|--------|-------------|
| `/org/gnome/mutter/` | `org.gnome.mutter` | Compositor behavior (dynamic workspaces, edge-tiling, etc.) |
| `/org/gnome/desktop/wm/preferences/` | `org.gnome.desktop.wm.preferences` | Window manager preferences (workspace count, focus mode, etc.) |
| `/org/gnome/shell/keybindings/` | `org.gnome.shell.keybindings` | GNOME Shell keybindings (screenshots, notifications, etc.) |
| `/org/gnome/desktop/interface/` | `org.gnome.desktop.interface` | Desktop interface settings (hot corners, etc.) |
| `~/.config/dconf/user` | — | Binary dconf database file (all settings) |

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

## Schema Reference: org.gnome.mutter

Controls the Mutter window manager/compositor behavior.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `dynamic-workspaces` | `b` | `true` | Workspaces are created/destroyed on demand |
| `edge-tiling` | `b` | `true` | Enable window tiling when dragged to screen edges |
| `workspaces-only-on-primary` | `b` | `true` | Workspaces only on primary display |
| `center-new-windows` | `b` | `false` | Center newly opened windows on screen |
| `auto-maximize` | `b` | `true` | Auto-maximize windows that are nearly screen-sized |
| `attach-modal-dialogs` | `b` | `false` | Attach modal dialogs to their parent window |
| `overlay-key` | `s` | `'Super_L'` | Key that triggers the Activities overlay |
| `focus-change-on-pointer-rest` | `b` | `true` | Change focus after pointer stops moving |
| `draggable-border-width` | `i` | `10` | Width of invisible border for resizing (pixels) |
| `locate-pointer-key` | `s` | `'Control_L'` | Key to trigger pointer location animation |
| `check-alive-timeout` | `u` | `5000` | Timeout (ms) before showing "not responding" dialog |
| `experimental-features` | `as` | `[]` | Experimental features to enable (e.g., `scale-monitor-framebuffer`) |

## Schema Reference: org.gnome.desktop.wm.preferences

Controls window manager preferences and workspace configuration.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `num-workspaces` | `i` | `4` | Number of workspaces (when dynamic-workspaces is false) |
| `workspace-names` | `as` | `[]` | Custom names for workspaces |
| `focus-mode` | `s` | `'click'` | Focus mode: `click`, `sloppy`, or `mouse` |
| `auto-raise` | `b` | `false` | Auto-raise focused windows (for sloppy/mouse focus) |
| `auto-raise-delay` | `i` | `500` | Delay (ms) before auto-raising |
| `raise-on-click` | `b` | `true` | Raise window when clicked |
| `action-double-click-titlebar` | `s` | `'toggle-maximize'` | Double-click titlebar action: `toggle-maximize`, `toggle-shade`, `minimize`, `none` |
| `action-middle-click-titlebar` | `s` | `'lower'` | Middle-click titlebar action |
| `action-right-click-titlebar` | `s` | `'menu'` | Right-click titlebar action |
| `button-layout` | `s` | `'appmenu:close'` | Titlebar button layout (Ubuntu default: `close,minimize,maximize:`) |
| `titlebar-font` | `s` | `'Cantarell Bold 11'` | Titlebar font |
| `titlebar-uses-system-font` | `b` | `true` | Use system font for titlebars |
| `resize-with-right-button` | `b` | `true` | Use right button (instead of middle) for resize |
| `mouse-button-modifier` | `s` | `'<Super>'` | Modifier key for mouse window actions |
| `audible-bell` | `b` | `true` | Enable audible bell |
| `visual-bell` | `b` | `false` | Enable visual bell (screen flash) |
| `visual-bell-type` | `s` | `'fullscreen-flash'` | Visual bell type: `fullscreen-flash` or `frame-flash` |
| `theme` | `s` | `'Adwaita'` | Window manager theme name |

## Schema Reference: org.gnome.shell.keybindings

Controls GNOME Shell-level keybindings. All values are string arrays (`as`).

| Key | Default | Description |
|-----|---------|-------------|
| `show-screenshot-ui` | `['<Shift>Print']` | Open the interactive screenshot tool |
| `screenshot` | `['Print']` | Take a full-screen screenshot |
| `screenshot-window` | `['<Alt>Print']` | Screenshot the active window |
| `show-screen-recording-ui` | `['<Ctrl><Shift><Alt>R']` | Open screen recording UI |
| `focus-active-notification` | `['<Super>n']` | Focus the active notification |
| `toggle-application-view` | `['<Super>a']` | Show/hide the application grid |
| `toggle-message-tray` | `['<Super>v']` | Show/hide the notification/calendar tray |
| `toggle-overview` | `[]` (disabled) | Toggle Activities overview |
| `toggle-quick-settings` | `[]` (disabled) | Toggle quick settings panel |
| `switch-to-application-1` | `['<Super>1']` | Launch/focus 1st pinned app |
| `switch-to-application-2` | `['<Super>2']` | Launch/focus 2nd pinned app |
| `switch-to-application-3` | `['<Super>3']` | Launch/focus 3rd pinned app |
| `switch-to-application-4` | `['<Super>4']` | Launch/focus 4th pinned app |
| `switch-to-application-5` | `['<Super>5']` | Launch/focus 5th pinned app |
| `switch-to-application-6` | `['<Super>6']` | Launch/focus 6th pinned app |
| `switch-to-application-7` | `['<Super>7']` | Launch/focus 7th pinned app |
| `switch-to-application-8` | `['<Super>8']` | Launch/focus 8th pinned app |
| `switch-to-application-9` | `['<Super>9']` | Launch/focus 9th pinned app |

> **Note:** The `switch-to-application-N` keys conflict with `switch-to-workspace-N` if both are set. Disable one set before enabling the other.

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

## Example dconf Dump

Export and import your desktop/workspace settings as a portable text file:

```ini
# Export: dconf dump /org/gnome/mutter/ > mutter.ini
# Import: dconf load /org/gnome/mutter/ < mutter.ini

[/]
dynamic-workspaces=false
edge-tiling=true
workspaces-only-on-primary=false
center-new-windows=true
auto-maximize=true
attach-modal-dialogs=false
overlay-key='Super_L'
check-alive-timeout=uint32 5000
experimental-features=@as []

[keybindings]
toggle-tiled-left=['<Super>Left']
toggle-tiled-right=['<Super>Right']
```

```ini
# Export: dconf dump /org/gnome/desktop/wm/preferences/ > wm-prefs.ini
# Import: dconf load /org/gnome/desktop/wm/preferences/ < wm-prefs.ini

[/]
num-workspaces=4
workspace-names=['Main', 'Code', 'Web', 'Chat']
focus-mode='click'
auto-raise=false
action-double-click-titlebar='toggle-maximize'
button-layout='close,minimize,maximize:'
titlebar-uses-system-font=true
mouse-button-modifier='<Super>'
resize-with-right-button=true
audible-bell=false
visual-bell=false
```

```ini
# Export: dconf dump /org/gnome/shell/keybindings/ > shell-keybindings.ini
# Import: dconf load /org/gnome/shell/keybindings/ < shell-keybindings.ini

[/]
show-screenshot-ui=['<Shift>Print']
screenshot=['Print']
screenshot-window=['<Alt>Print']
show-screen-recording-ui=['<Ctrl><Shift><Alt>R']
focus-active-notification=['<Super>n']
toggle-application-view=['<Super>a']
toggle-message-tray=['<Super>v']
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
gsettings reset-recursively org.gnome.shell.keybindings
```

> **Tip:** Use `dconf watch /` in a terminal to see real-time changes as you modify settings through the GUI.
