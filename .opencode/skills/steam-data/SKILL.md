---
name: steam-data
description: Steam metadata, save path conventions, and Proton path handling
compatibility: opencode
---

# Steam Data Rules

## Steam metadata format
Steam `.vdf` and `.acf` files are not JSON.
Use the `vdf` Python package to parse them.

## Steam library file
Path:
`<steam_path>/steamapps/libraryfolders.vdf`

Purpose:
- contains Steam library locations
- each library can contain its own `steamapps/` directory
- library path discovery starts here

## App manifest files
Path:
`<library_path>/steamapps/appmanifest_<appid>.acf`

Important fields:
- `appid`
- `name`
- `installdir`

## Save path strategy
Do not assume save files are inside the installation directory.
Search order should be:
1. explicit rule from `data/game_rules.json`
2. environment-variable expansion
3. validation of path existence
4. conservative heuristic search
5. manual user override

## Typical Windows save roots
- `%USERPROFILE%/Saved Games/<Game>`
- `%APPDATA%/<Publisher>/<Game>`
- `%LOCALAPPDATA%/<Game>`
- `%USERPROFILE%/Documents/My Games/<Game>`
- `%USERPROFILE%/AppData/LocalLow/<Publisher>/<Game>`
- occasionally near install directory

## Proton path model on Steam Deck
Base prefix:
`~/.local/share/Steam/steamapps/compatdata/<appid>/pfx/drive_c/`

Typical translation:
- `C:/Users/<user>/Saved Games/X`
  -> `users/steamuser/Saved Games/X`
- `C:/Users/<user>/AppData/Roaming/X`
  -> `users/steamuser/AppData/Roaming/X`
- `C:/Users/<user>/AppData/Local/X`
  -> `users/steamuser/AppData/Local/X`
- `C:/Users/<user>/Documents/My Games/X`
  -> `users/steamuser/Documents/My Games/X`

## Native Linux save paths
Some games use native Linux paths instead of Proton:
- `~/.local/share/<game>`
- `~/.config/<game>`

## Save file extensions
Common save-related extensions include:
- `.sav`
- `.save`
- `.dat`
- `.sl2`
- `.json`
- `.bin`
- `.profile`
- `.db`

Treat these as hints, not guarantees.

## Heuristic rules
- Heuristics must be conservative
- If confidence is weak, return an unverified result
- Do not present guessed paths as verified paths
