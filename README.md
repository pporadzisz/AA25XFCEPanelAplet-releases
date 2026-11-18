# 25AApanel

An XFCE4 panel plugin that displays live game-server statistics:
**Online | In-Game | Servers**. Data comes from a UDP server query and a
server list URL. Hover for a tooltip with server details, left-click to run
a command (e.g. launch the game), and optional desktop notifications when a
mission becomes joinable.

Official packages are attached to each [release](https://github.com/pporadzisz/25aapanel-releases/releases).

## Installation

Download the package for your distribution from the
[latest release](https://github.com/pporadzisz/25aapanel-releases/releases/latest)
(file names contain the version number), then run the matching command.

### Debian / Ubuntu (amd64)

```bash
sudo apt install ./25aapanel_*_amd64.deb
```

`apt` resolves runtime dependencies (`xfce4-panel`, `libgtk-3-0`, `libnotify4`,
`libcurl4`) automatically.

### Fedora (x86_64)

```bash
sudo dnf install ./25aapanel-*.rpm
```

### Arch Linux (x86_64)

```bash
sudo pacman -U ./25aapanel-*.pkg.tar.zst
```

## After installation

1. Restart the panel: `xfce4-panel -r`
2. Add the **25AApanel** item: Panel → Add New Items…
3. The plugin shows `Online | In-Game | Servers` and updates periodically.

## Configuration

Right-click the plugin and select **Properties**, or edit
`~/.config/xfce4/panel/25AApanel/config`:

- **refresh_interval** — seconds between data updates (default: 60)
- **command** — executable run on left-click (e.g. `steam steam://rungameid/123`)
- **notify_enabled** — `true`/`false` for desktop notifications
- **min_players** — minimum players for the notification trigger (default: 3)
- **mission_name** — mission name substring (case-insensitive) for
  notifications (default: `Pipeline`)

Changes take effect on the next data update.
