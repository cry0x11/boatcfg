# Boatcfg
A comfy boat in the river

## List of dependencies
- `eww` main bar
- `wideriver` tiling manager for river
- `ristate` to integrate eww with river
- `lswt` for riverprop script (shows you title and id of focused window)
- `jq` parses lswt output (used in riverprop script)
- `wf-recorder` for screen recording
- `wl-clipboard` wayland clipboard
- `wl-randr` used to get resolution for centering rules in river config
- `gawk` needed for processing of text in river config and some shell scripts

### Optional
- `cliphist` history for clipboard. Used in `~/.config/river/autostart` and `~/.scripts/clipboard`
- `gammastep` night read mode. Used in `~/.config/river/autostart`
- `swaybg` for setting wallpapers. Used in `~/.config/river/autostart`
- `lf` terminal file manager. Used in `~/.scripts/fileManager`
- `kdeconnect` connects your phone with the system. Required if you intend to use `~/.scripts/kcmount`
- `sshfs` for kdeconnect mount script. Required for `~/.scripts/kcmount`
- `dunst` notification daemon. I used `notify-send` in scripts so I guess it's not required (Warning: pango markup is used in scripts).
- `foot` terminal emulator. Used in `~/.scripts/fileManager` and `~/.config/river/riverprop`

## Installing dependencies
### Void linux
#### Repo
Here's the install command for most of the above. Note that `doas` is a minimal `sudo` alternative, just use `sudo` instead if you already have it.

```Bash
doas xbps-install -S cliphist lf gawk wlr-randr wl-clipboard kdeconnect
```

#### Compiling
1. River itself requires compiling as the void package is old. This is because zig wasn't updated in the main void repos as of the time writing this guide (zig ver. 10 and river requires zig ver. 11)

### Arch linux
Same or similar package names are present in arch repositories and you don't need to compile software yourself since you have the AUR

## Preview
https://github.com/cry0x11/boatcfg/assets/167573046/8eee081b-73ea-4b68-9533-8cddfd04aad9
