# REAPER ↔ Omarchy theme integration

简体中文版见 [README_ZH.md](README_ZH.md)

One screenshot first:

<img src="picture/tokyo-night.png" width="80%">

## Usage

```bash
omarchy hook install theme-set ./reaper-omarchy-theme
omarchy theme set "$(omarchy theme current)"
```

## How it works

The hook reads the active Omarchy palette (`colors.toml`) and writes to two
**native REAPER interfaces**:

| Native REAPER interface | Covers | Takes effect |
|---|---|---|
| `.ReaperTheme` (`col_*` colors) | Arrange view · Transport · Lists · MIDI editor | Live reload on theme change |
| `libSwell-user.colortheme` | SWELL native widgets (FX browser · Menus · Dialogs) | On next REAPER launch (a notification reminds you) |

## Generated files

- `~/.config/REAPER/ColorThemes/Omarchy.ReaperTheme`
- `~/.config/REAPER/Scripts/Omarchy/load-omarchy-theme.lua` — live-reload loader
- `~/.config/REAPER/libSwell-user.colortheme`

## Screenshots

### metta-black
<img src="picture/metta-black.png" width="80%">

### rose-pine
<img src="picture/rose-pine.png" width="80%">

### tokyo-night
<img src="picture/tokyo-night.png" width="80%">

### vetablack
<img src="picture/vetablack.png" width="80%">

### SWELL update notification
<img src="picture/notification.png" width="40%">

## Credits

The live-reload framework comes from
[nofatetech/reaper-omarchy-theme](https://github.com/nofatetech/reaper-omarchy-theme)
(the SWELL coloring and the notification part were contributed upstream as
[PR #1](https://github.com/nofatetech/reaper-omarchy-theme/pull/1)).

This fork adds a few **personal tweaks** on top of it: contrast interpolation
parameters and hardcoded font size/layout values. Nothing fancy — it just fits
my own setup.
