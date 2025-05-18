# i3 Configuration – Overview & Commentary

This document walks through the key pieces of my **i3 window‑manager setup**.  
The goal is to make the config self‑documenting so I—or anyone who clones the repo—can tweak it confidently. Most of this comes standard with the basic i3 Config with a few personal tweaks to fit my style and workflow.

![image](https://github.com/user-attachments/assets/4c447258-9b8e-409b-8c43-93d68e3cec30)

---

## Table of Contents

1. [Basics](#basics)
2. [Display & Wallpaper](#display--wallpaper)
3. [Autostarted Utilities](#autostarted-utilities)
4. [Audio Key‑bindings](#audio-key-bindings)
5. [Window Management](#window-management)
6. [Application Launchers](#application-launchers)
7. [Workspaces](#workspaces)
8. [Resize Mode](#resize-mode)
9. [Polybar](#polybar)
10. [Maintenance Shortcuts](#maintenance-shortcuts)

---

## Basics

| Setting                  | Purpose                                                                                                                                                    |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `set $mod Mod4`          | Uses the **Super/Windows** key as the main modifier. This variable is referenced everywhere as `$mod` so switching to `Mod1` (Alt) later is one‑line work. |
| `font pango:monospace 8` | Default monospace font for window titles and the bar.                                                                                                      |

---

## Display & Wallpaper

```i3
exec --no-startup-id xrandr --output Virtual-1 --mode 1920x1080 --rate 60
exec --no-startup-id nitrogen --restore
````

* Forces the monitor named **`Virtual-1`** to 1920 × 1080 @ 60 Hz on every login/reload.
	* It is defaulted to this as I am building this in a VM, feel free to change this to match your display name
* Restores the last wallpaper chosen in **Nitrogen**.

> **Tip:** if the output name changes (e.g., `eDP-1`, `HDMI-1`), update the `xrandr` line.

---

## Autostarted Utilities

| Command             | Reason                                                               |
| ------------------- | -------------------------------------------------------------------- |
| `dex --autostart`   | Runs XDG‑compliant `.desktop` files placed in `~/.config/autostart`. |
| `xss-lock … i3lock` | Locks the screen automatically before suspend.                       |
| `nm-applet`         | System‑tray GUI for NetworkManager.                                  |

---

## Audio Key‑bindings

PulseAudio volume control plus auto‑refresh of `i3status`:

```i3
set $refresh_i3status killall -SIGUSR1 i3status
bindsym XF86AudioRaiseVolume exec … +10% && $refresh_i3status
…
```

* Hardware **volume keys** adjust `@DEFAULT_SINK@`.
* Microphone mute toggle bound to **`XF86AudioMicMute`**.

---

## Window Management

### Focus & Movement

| Action                   | Keys                               |
| ------------------------ | ---------------------------------- |
| Focus left/right/up/down | `$mod+j/k/l/;` or arrow keys       |
| Move window              | `$mod+Shift+j/k/l/;` or arrow keys |

### Layouts & Modes

| Action                  | Key                 | Note                               |
| ----------------------- | ------------------- | ---------------------------------- |
| Split **h** / **v**     | `$mod+h` / `$mod+v` | Horizontal vs. vertical containers |
| Toggle floating         | `$mod+Shift+t`      |                                    |
| Fullscreen              | `$mod+f`            |                                    |
| Tiling ↔ Floating focus | `$mod+space`        |                                    |

### Gaps

```i3
gaps inner 15
gaps outer 15
```

Creates a 15 px margin between windows (inner) and screen edge (outer).

---

## Application Launchers

* **Terminal:** `$mod+Return` → `i3-sensible-terminal`
* **Rofi app launcher:** `$mod+/`
* **Rofi window switcher:** `$mod+c`

---

## Workspaces

Variables `"$ws1"` … `"$ws10"` prevent duplication.

| Switch to                                       | Move window to                  |
| ----------------------------------------------- | ------------------------------- |
| `$mod+1` … `$mod+0`                             | `$mod+Shift+1` … `$mod+Shift+0` |
| **Next/Prev workspace**: `$mod+Ctrl+Right/Left` |                                 |

---

## Resize Mode

Press `$mod+r` to enter **resize** sub‑mode:

| Key                            | Effect                                  |
| ------------------------------ | --------------------------------------- |
| `j/k/l/;` or arrows            | Grow/Shrink width/height in 10 px steps |
| `Return` / `Escape` / `$mod+r` | Return to default mode                  |

---

## Polybar

```i3
exec_always --no-startup-id ~/.config/polybar/launch.sh
```

Runs a custom script that spawns Polybar on every reload (ensures old instances close first).

---

## Maintenance Shortcuts

| Keys           | Command                                    |
| -------------- | ------------------------------------------ |
| `$mod+Shift+c` | **Reload** i3 config (`i3-msg reload`)     |
| `$mod+Shift+r` | **Restart** i3 in‑place (`i3-msg restart`) |
| `$mod+Shift+e` | Confirmation dialog, then **exit** i3      |

---

### Planned for Future Tweaks

1. **Add screen‑record / screenshot bindings** the same way—see README notes in the main repo.
2. **Multiple monitors:** replicate or script additional `xrandr` lines, or switch to *autorandr*.
3. **Wayland (sway)**: most bindings stay the same, but display/layout syntax changes—keep this file as reference.

