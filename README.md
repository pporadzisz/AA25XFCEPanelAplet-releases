# 25AApanel
[![GitHub all releases](https://shields.io)](https://github.com)

An XFCE4 panel plugin that displays live game-server statistics:
**Online | In-Game | Servers**. Data comes from a UDP server query and a
server list URL. Hover for a tooltip with server details, left-click to run
a command (e.g. launch the game), and optional desktop notifications when a
mission becomes joinable.

Official packages are attached to each [release](https://github.com/pporadzisz/AA25XFCEPanelAplet-releases/releases).

## Tested on

| OS | Version | Package |
|---|---|---|
| Xubuntu | 24.04,26.04 | `.deb` |
| Debian Xfce| 13.7 | `.deb` |
| Linux Mint Xfce| 26.04 | `.deb` |
| MX-Linux Xfce | 25.3 | `.deb`|
| Fedora Xfce | 45 | `rpm`|

More distributions will be added as they are tested.

## Installation

Download the package for your distribution from the
[latest release](https://github.com/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest)
(file names contain the version number), then run the matching command.

### From the terminal with `curl` (no browser needed)

These snippets query the GitHub API for the newest release, pick the matching
package, download it and install it. They keep working when a new version is
published — no manual version/tag lookup required. Requires `curl` and the
usual `grep`/`head` coreutils.

**Debian / Ubuntu (amd64):**

```bash
DEB_URL=$(curl -fsSL https://api.github.com/repos/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest \
  | grep -o 'https://[^"]*_amd64\.deb' | head -n1)
curl -fLO "$DEB_URL"
sudo apt install "./$(basename "$DEB_URL")"
```

**Fedora (x86_64):**

```bash
RPM_URL=$(curl -fsSL https://api.github.com/repos/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest \
  | grep -o 'https://[^"]*\.x86_64\.rpm' | grep -v -- '-debug-' | head -n1)
curl -fLO "$RPM_URL"
sudo dnf install "./$(basename "$RPM_URL")"
```

**Arch Linux (x86_64):**

```bash
PKG_URL=$(curl -fsSL https://api.github.com/repos/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest \
  | grep -o 'https://[^"]*\.pkg\.tar\.zst' | grep -v -- '-debug-' | head -n1)
curl -fLO "$PKG_URL"
sudo pacman -U "./$(basename "$PKG_URL")"
```

### The same with `wget`

Use `wget -qO-` instead of `curl -fsSL` for the API query and `wget "$URL"`
instead of `curl -fLO "$URL"` for the download. For example, on Debian/Ubuntu:

```bash
DEB_URL=$(wget -qO- https://api.github.com/repos/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest \
  | grep -o 'https://[^"]*_amd64\.deb' | head -n1)
wget "$DEB_URL"
sudo apt install "./$(basename "$DEB_URL")"
```

For Fedora/Arch keep the `grep -o 'https://[^"]*\.x86_64\.rpm'` /
`grep -o 'https://[^"]*\.pkg\.tar\.zst'` patterns from above, then install with
`sudo dnf install` / `sudo pacman -U` as shown.

> If the URL variable ends up empty, the latest release has no matching package
> for your distribution/architecture — check the
> [release assets](https://github.com/pporadzisz/AA25XFCEPanelAplet-releases/releases/latest)
> directly.

### Manual download + install

If you downloaded a package yourself, install the local file with:

**Debian / Ubuntu (amd64):**

```bash
sudo apt install ./25aapanel_*_amd64.deb
```

`apt` resolves runtime dependencies (`xfce4-panel`, `libgtk-3-0`, `libnotify4`,
`libcurl4`) automatically.

**Fedora (x86_64):**

```bash
sudo dnf install ./25aapanel-*.rpm
```

**Arch Linux (x86_64):**

```bash
sudo pacman -U ./25aapanel-*.pkg.tar.zst
```

### Reinstalling the same version

If `25aapanel` with the same version is already installed, plain `apt install`
reports that it is already the newest version and does nothing. Force a
reinstall with:

```bash
sudo apt install --reinstall ./25aapanel_1.0_amd64.deb
```

Use the file name you actually downloaded — the version in the example above is
just that: an example. A wildcard works as well, e.g.
`sudo apt install --reinstall ./25aapanel_*_amd64.deb`.

## After installation

1. Restart the panel: `xfce4-panel -r`
2. Add the **25AApanel** item: Panel → Add New Items…
3. The plugin shows `Online | In-Game | Servers` and updates periodically.

## Usage

- **Left-click** opens a small menu with **Run AA 2.5** (plugin icon on the
  left), which launches the configured command.
- **Right-click** opens the standard panel menu (Properties, About).
- In **Properties**, the **Find AA25 Launcher** button searches for
  `AA25Assist-*.AppImage` (`~/app`, `~/Applications`, `~/Downloads`, `~/Desktop`,
  then the home directory, `/opt` and `/usr/local`) and fills the Command field
  automatically. A spinner is shown while the search runs in the background.

## Configuration

Right-click the plugin and select **Properties**, or edit
`~/.config/25AApanel/config`:

- **refresh_interval** — seconds between data updates (default: 60)
- **command** — executable run on left-click / Run AA 2.5 (e.g. the
  `AA25Assist-*.AppImage` path; use the **Find AA25 Launcher** button)
- **notify_enabled** — `true`/`false` for desktop notifications; muted
  automatically while the 25Assist launcher/game is running and resumed after it
  exits (the setting itself is unchanged)
- **min_players** — minimum players for the notification trigger (default: 3)
- **mission_name** — mission for the notification trigger, selected from the
  built-in mission list (default: `Pipeline`); matching is case-insensitive
- **font_color** — text colour for the panel label and tooltip in `#rrggbb`
  form (default: `#008000`, bottle green, as in previous releases). In
  Properties, **Use theme text color** is unchecked by default, so the colour
  picker is active; check it (stored as an empty value) to let GTK use the
  theme's foreground — helpful when a bright tooltip background makes green hard
  to read (e.g. Linux Mint).
- **font_size** — font size in points for the panel label and tooltip (default:
  `0`, i.e. inherit the theme/panel font size, as before). **Use theme font size**
  is checked by default in Properties; uncheck it to enable the spin button
  (6–48 pt).

Each matching mission triggers a notification with a 12-second timeout; the
check runs on every refresh (no rate limit). Some notification daemons override
the requested timeout with their own setting.

Changes take effect on the next data update.

## Uninstall

Remove the package with your distribution's package manager:

**Debian / Ubuntu:**

```bash
sudo apt remove 25aapanel
```

**Fedora:**

```bash
sudo dnf remove 25aapanel
```

**Arch Linux:**

```bash
sudo pacman -R 25aapanel
```

The `.deb` package restarts a running panel automatically; otherwise run
`xfce4-panel -r` yourself. Your settings are kept in
`~/.config/25AApanel/config` — delete that file too if you want a completely
clean install (`apt purge` does not remove user configuration).

[![GitHub all releases](https://shields.io)](https://github.com)

