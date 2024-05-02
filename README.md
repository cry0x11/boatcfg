# Boatcfg
A comfy boat in the river

## Compiling dependencies
`river` package is out of date in void repos because `zig` is also out of date
### Acquiring zig
Zig on Void linux is out of date as of the time I'm writing this guide (ver. 10) and version 11 is required to compile river correctly



### Wideriver
Wideriver is by far the most feature rich tiling manager for river I have found
```Bash
git clone clone git@github.com:alex-courtis/wideriver.git
cd wideriver
make
doas make install
```

### Ristate
1. Clone and build
```Bash
git clone https://gitlab.com/snakedye/ristate.git
cd ristate
cargo build --release
```

2. Make executable and move to `/usr/local/bin`
```Bash
cd target/release
chmod +x ./ristate
doas cp ristate /usr/local/bin
```

### Lswt (Required for riverprop script)
I hate the sourcehut git interface and find it confusing and painful.
```Bash
git clone https://git.sr.ht/~leon_plickat/lswt
cd lswt
make
doas make install
```

### EWW
1. Install rustup.
```Bash
doas xbps-install -S rustup
```

2. Run it and it will install `rustc` and `cargo`
```Bash
rustup-init
```

3. Install dependencies
```Bash
doas xbps-install -S
```

4. Clone EWW and compile it for wayland
```Bash
git clone https://github.com/elkowar/eww
cd eww
cargo build --release --no-default-features --features=wayland
```

5. Make resulted binary executable
```Bash
cd target/release
chmod +x ./eww
```

6. Copy EWW bin to `/usr/local/bin` 
```Bash
doas cp eww /usr/local/bin/
```


















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
