# REAPER ↔ Omarchy theme integration

Automatically syncs REAPER's editable UI colors with the active
[Omarchy](https://omarchy.org/) theme while preserving REAPER's familiar stock
layout and control assets.

`reaper-omarchy-theme` is an Omarchy `theme-set` hook. It:

- reads the active Omarchy `colors.toml` palette;
- creates `~/.config/REAPER/ColorThemes/Omarchy.ReaperTheme`;
- reuses the installed REAPER default theme's image and WALTER layout assets;
- enables REAPER's dark miscellaneous-window mode;
- recolors SWELL's native widgets (FX browser tree, filter boxes, menus,
  device dialogs, ...) via `libSwell-user.colortheme`;
- selects the generated theme for the next REAPER launch; and
- reloads it live through ReaScript when REAPER is already running; and
- when the SWS extension is installed, reloads the full theme (including
  bitmap assets) live through SWS theme resources.

The archive discovery deliberately drains `unzip` output completely so the
hook remains reliable with large REAPER theme archives under `pipefail`.

## Requirements

- Omarchy 4 with `colors.toml`-based themes
- REAPER for Linux with a stock `Default_*.ReaperThemeZip`
- Bash, `awk`, `find`, `sort`, and `unzip`

## Install

From this repository:

```bash
omarchy hook install theme-set ./reaper-omarchy-theme
omarchy theme set "$(omarchy theme current)"
```

The second command applies the integration immediately. After that, changing
the Omarchy theme regenerates REAPER's `Omarchy.ReaperTheme` automatically.

If REAPER is open, the hook asks its supported ReaScript API to reload the
theme. If REAPER is closed, the generated theme is selected for its next
launch through `reaper.ini`.

## What changes

The hook creates or updates:

- `~/.config/REAPER/ColorThemes/Omarchy.ReaperTheme`
- `~/.config/REAPER/Scripts/Omarchy/load-omarchy-theme.lua`
- `~/.config/REAPER/libSwell-user.colortheme`
- REAPER's `lastthemefn5` setting when REAPER is closed

It installs as:

- `~/.config/omarchy/hooks/theme-set.d/reaper-omarchy-theme`

Existing Omarchy hooks are left untouched.

## Limitations

REAPER themes combine editable color definitions with bitmap control assets.
This integration recolors the arrange view, timeline, selections, cursors,
lists, MIDI editor, meters, routing indicators, and related surfaces. Gray
track and mixer controls supplied by REAPER's stock bitmap assets remain gray.

Live reloading has one additional caveat: REAPER caches theme bitmaps, so the
native ReaScript path (`OpenColorThemeFile`) refreshes colors immediately but
bitmap assets only apply on the next launch. If you use the
[SWS extension](https://www.sws-extension.org/) and register the generated
theme as a theme resource once (SWS Resources window, Theme tab), the hook
detects SWS and reloads through it, which re-reads the theme file from disk
including bitmap assets.

`libSwell-user.colortheme` is read by REAPER at startup, so SWELL widget
colors always apply on the next launch.

Audio devices and REAPER project preferences are not changed.

## Uninstall

```bash
rm ~/.config/omarchy/hooks/theme-set.d/reaper-omarchy-theme
```

After uninstalling, choose another theme from REAPER's **Options → Themes** menu.
