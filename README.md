# Boatcfg
A comfy boat in the river

## Features
River is already capable but I created bash functions to make it easier
1. Keychords
2. Easy rules `float`, `dimensions` and `positions`
3. Start programs on specific tag
4. EWW integration

## Dependencies

### Arch Linux
1. Refer to command below in **Void Linux** section to see required software and install it with `pacman`
2. `zig` and `river` are up to date in the core repos so you don't need to compile them
3. You also have the AUR, so you can install what you're missing from there and skip steps below to compile software

### Void Linux
- I use Void btw
- Commands below use `doas` which is a minimal alternative to `sudo`. Just use `sudo` like you do and don't mind me.

#### Core (Required)
- `wl-randr` used to get resolution for centering rules in river config
- `gawk` needed for processing of text in river config and some shell scripts
- `elogind` for the peace of mind as river requires XDG\_RUNTIME\_DIR (Arch users already have systemd)
- `dbus` you need dbus for EWW systray to work
- `jq` parses lswt output (used in riverprop script to obtain **APP-ID** and **Title** of currently focused window)
- `seatd` river expects seatd, so why not?

```Bash
doas xbps-install -S dbus elogind gawk wlr-randr jq seatd
```

- `river`, `wideriver`, `eww`, `ristate` and `lswt` are in the **Compiling dependencies** section

#### Optional
- `gammastep` night read mode. Used in `~/.config/river/autostart`
- `swaybg` for setting wallpapers. Used in `~/.config/river/autostart`
- `wf-recorder` for screen recording. Used in `~/.scripts/shotcopy`
- `lf` terminal file manager. Used in `~/.scripts/fileManager`
- `wl-clipboard` wayland clipboard. Used in `~/.scripts/clipboard`
- `cliphist` history for clipboard. Used in `~/.config/river/autostart` and `~/.scripts/clipboard`
- `foot` terminal emulator. Used in `~/.scripts/fileManager` and `~/.config/river/riverprop`
- `dunst` notification daemon. I used `notify-send` in scripts so I guess it's not required (Warning: pango markup is used in scripts).
- `curl` used in `~/.scripts/prayer-ical-parser`
- `sshfs` for kdeconnect mount script. Required for `~/.scripts/kcmount`
- `kdeconnect` connects your phone with the system. Required if you intend to use `~/.scripts/kcmount`
- `grim` screenshot tool. Used in `~/.scripts/shotcopy`
- `slurp` region selector tool. Used in `~/.scripts/shotcopy`

```Bash
doas xbps-install -S cliphist lf gammastep swaybg wf-recorder foot dunst curl fuse-sshfs kdeconnect grim slurp wl-clipboard
```

### Compiling dependencies
- `river` Obviously, it's the main thing
- `eww` main bar
- `wideriver` tiling manager for river. it provides all the functionality I need.
- `ristate` to integrate eww with river
- `lswt` for riverprop script (shows you title and id of focused window)

NOTE: `river` package is out of date in void repos because `zig` is also out of date, so I'll begin with acquiring zig and compiling river

#### Acquiring zig
Zig on Void linux is out of date as of the time I'm writing this guide (ver. 10) and version 11 is required to compile river correctly
```Bash
wget https://ziglang.org/download/0.11.0/zig-linux-x86_64-0.11.0.tar.xz
tar -xf zig-linux-x86_64-0.11.0.tar.xz
```

#### Compiling River
1. Install dependencies
```Bash
doas xbps-install -S base-devel wayland wayland-protocols wlroots libevdev-devel pixman pkg-config scdoc libxkbcommon-devel wayland-devel wlroots-devel xorg-server-xwayland mesa-dri
```

2. Clone and build
Remember to replace `<ZIG-DIR>` with the directory where you extracted the zig archive downloaded earlier
```Bash
git clone https://codeberg.org/river/river
cd river
git submodule update --init
<ZIG-DIR>/zig-linux-x86_64-0.11.0/zig build -Dxwayland -Doptimize=ReleaseSafe --prefix ~/.local install
```

3. Make executable and move to `/usr/local/bin`
```Bash
cd ~/.local/bin
chmod +x river*
cp river* /usr/local/bin
```

4. Moving man pages to system
```Bash
cd ~/.local/share/man/man1/
doas cp river* /usr/share/man/man1/
```

#### Wideriver
```Bash
git clone clone git@github.com:alex-courtis/wideriver.git
cd wideriver
make
doas make install
```

#### Ristate
1. Clone repo and build `ristate`
```Bash
git clone https://gitlab.com/snakedye/ristate.git
cd ristate
cargo build --release
```

2. Make `ristate` executable and move it to `/usr/local/bin`
```Bash
cd target/release
chmod +x ./ristate
doas cp ristate /usr/local/bin
```

#### Lswt
```Bash
git clone https://git.sr.ht/~leon_plickat/lswt
cd lswt
make
doas make install
```

#### EWW
1. Install rustup.
```Bash
doas xbps-install -S rustup 
```

2. Go through the installer to get `rustc` and `cargo`
```Bash
rustup-init
```

3. Install dependencies
```Bash
doas xbps-install -S gtk-layer-shell gtk-layer-shell-devel glib-devel libdbusmenu-gtk3 libdbusmenu-gtk3-devel libgtk-3 gdk-pixbuf2 cairo-devel pango-devel glib-2 glib-devel libgcc libgcc-devel
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

## Post-installation
1. Add user to groups
```Bash
doas usermod -aG audio,video,input,_seatd <USERNAME>
```

2. Enable services
```Bash
doas ln -s /etc/sv/seatd/ /etc/runit/runsvdir/default/
doas ln -s /etc/sv/dbus/ /etc/runit/runsvdir/default/
doas ln -s /etc/sv/elogind/ /etc/runit/runsvdir/default/
```

3. Whenever you run `river` use `dbus-run-session` for systray in EWW to work
```Bash
dbus-run-session river
```
