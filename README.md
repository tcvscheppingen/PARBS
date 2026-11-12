# PARBS
Penrose Auto Rice Bootstrapping Script: installs my window manager [penrosewm](https://github.com/tcvscheppingen/penrosewm), built with the [Penrose](https://github.com/sminez/penrose) library, and the rest of the desktop: st, dmenu, slock, dwmblocks and dunst in [gruvbox](https://github.com/morhetz/gruvbox) dark, the same colors as the penrose bar.

![Opening terminals, switching layouts and workspaces, and starting a program from dmenu](https://raw.githubusercontent.com/tcvscheppingen/penrosewm/main/demo.gif)

## Install

For Arch and Arch based distributions, Fedora, and Artix with dinit. **Read a script before you run it.** Run it as your own user, not as root; it uses `sudo` where needed.

```bash
git clone https://github.com/tcvscheppingen/PARBS
cd PARBS
./auto-rice.sh        # on Artix: ./auto-rice-artix.sh
```

Then reboot. The script:
- installs Rust with [rustup](https://rust-lang.org/learn/get-started/), and Xorg, xmodmap, PipeWire, NetworkManager, Bluetooth, fonts, Neovim, Thunar and LibreWolf, plus the Intel or AMD GPU drivers it finds (not NVIDIA)
- backs up the configs it replaces to `~/.config/penrose-backup-<date>/`
- copies the dotfiles, clones [st](https://github.com/tcvscheppingen/st), [dmenu](https://github.com/tcvscheppingen/dmenu), [slock](https://github.com/tcvscheppingen/slock) and [dwmblocks](https://github.com/tcvscheppingen/dwmblocks) into `~/.local/src`, builds them and installs them to `/usr/local`
- clones [penrosewm](https://github.com/tcvscheppingen/penrosewm) to `~/.local/src/penrose`, builds it and installs it as `/usr/local/bin/penrose`
- asks how to start penrose: on login on TTY1, or from a minimal LightDM login screen (`RICE_LOGIN=tty|dm|none` answers in advance)

Starting penrose from your own `.desktop` file, for example from SDDM next to KDE? Let it run `~/.config/x11/xinitrc`, not `penrose`: xinitrc sets the wallpaper, puts the status bar scripts on `PATH` and starts dwmblocks, dunst and the rest, then penrose.

Already have the packages? `./move-config-files.sh` only copies the dotfiles, clones penrose and the suckless programs and builds them. It needs `git`.

Penrose doesn't start? Run `./check-install.sh`. It prints one line per check, `ok` or `PROBLEM` with the cause.

The key bindings are in the [penrosewm README](https://github.com/tcvscheppingen/penrosewm#key-bindings).

## Good to know

- **Status bar:** the penrose bar shows the workspaces, the layout, the window title and, on the right, the dwmblocks text (network, recording, volume, battery, memory, clock) without icons. There is no system tray, and clicking a block does nothing: that needs dwm's statuscmd patch.
- **Log:** penrose writes to `~/.local/share/penrose.log`. Look there when it doesn't start.
- **Changing penrose:** see the [penrosewm README](https://github.com/tcvscheppingen/penrosewm#changing-penrose). The source is in `~/.local/src/penrose`.
- **Changing st, dmenu, slock or dwmblocks:** edit `config.h` in `~/.local/src/<program>`, then `make && sudo make install` there. Each is a clone of its own repo, so `git pull` there gets updates.
- **Screenshots and recordings:** `maimpick` and `dmenurecord` are installed without a key: run them from dmenu, or bind them in penrose's `src/main.rs`.
- **No sound?** If the volume shows `vol dummy` on an Intel laptop, run `sound-fix`.
- **Locking:** after 5 minutes idle, and before sleep. The displays turn off after 10 minutes. The times are the `xset` lines in `~/.config/x11/xinitrc`. Neither happens while a video or music plays (`keepawake`); a paused one doesn't count.
- **Wallpaper:** `setbg image.jpg` changes it, and `setbg folder` picks a random image from a folder.

## Palette

[gruvbox](https://github.com/morhetz/gruvbox) dark:

| | Color | |
|---|---|---|
| Background | `#282828` | Bar, terminal, menus |
| Border | `#3c3836` | Unfocused windows |
| Text | `#ebdbb2` | |
| Accent | `#458588` | Focused workspace, dmenu, notifications |
| Red | `#cc241d` / `#fb4934` | Wrong password / critical notifications |

The colors are in penrose's `src/main.rs`, in each program's `config.h` in `~/.local/src`, in `~/.config/dunst/dunstrc` and in `~/.config/nvim/colors/gruvbox.lua`. Helix uses its built-in `gruvbox` theme.

## Credits
- [Penrose](https://github.com/sminez/penrose) by sminez
- [suckless.org](https://suckless.org) for st, dmenu and slock, and Luke Smith's [st](https://github.com/LukeSmithxyz/st), [dwmblocks](https://github.com/LukeSmithxyz/dwmblocks) and [voidrice](https://github.com/LukeSmithxyz/voidrice) scripts
- [gruvbox](https://github.com/morhetz/gruvbox) by Pavel Pertsev
