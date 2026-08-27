# REAPER ↔ Omarchy theme integration

Automatically syncs REAPER's editable UI colors with the active
[Omarchy](https://omarchy.org/) theme while preserving REAPER's familiar stock
layout and control assets.

`reaper-omarchy-theme` is an Omarchy `theme-set` hook. It:

- reads the active Omarchy `colors.toml` palette;
- creates `~/.config/REAPER/ColorThemes/Omarchy.ReaperTheme`;
- reuses the installed REAPER default theme's image and WALTER layout assets;
- enables REAPER's dark miscellaneous-window mode;
- selects the generated theme for the next REAPER launch; and
- reloads it live through ReaScript when REAPER is already running.

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
- REAPER's `lastthemefn5` setting when REAPER is closed

It installs as:

- `~/.config/omarchy/hooks/theme-set.d/reaper-omarchy-theme`

Existing Omarchy hooks are left untouched.

## Limitations

REAPER themes combine editable color definitions with bitmap control assets.
This integration recolors the arrange view, timeline, selections, cursors,
lists, MIDI editor, meters, routing indicators, and related surfaces. Gray
track and mixer controls supplied by REAPER's stock bitmap assets remain gray.

Audio devices and REAPER project preferences are not changed.

## Uninstall

```bash
rm ~/.config/omarchy/hooks/theme-set.d/reaper-omarchy-theme
```

After uninstalling, choose another theme from REAPER's **Options → Themes** menu.
