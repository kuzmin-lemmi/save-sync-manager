---
name: gui-patterns
description: PySide6 GUI patterns for Save Sync Manager
compatibility: opencode
---

# GUI Patterns

## Priority
GUI is not the first milestone.
Only build GUI after core logic, CLI, and Deck agent are stable.

## Main architecture
- Use `QMainWindow`
- Main layout: sidebar + `QStackedWidget`
- Keep UI thin; business logic must stay in core/services modules
- Communicate through signals/slots

## Long-running tasks
Do not block the UI thread.
Use:
- `QThread`
- worker objects
- signals for progress, result, and error reporting

## Required screens
- `GameListScreen`
- `GameProfileScreen`
- `CompareScreen`
- `SettingsScreen`
- `LogScreen`
- `SetupWizard`

## Status colors
- `identical`: `#4CAF50`
- `pc_newer`: `#2196F3`
- `deck_newer`: `#FF9800`
- `conflict`: `#F44336`
- `error`: `#9E9E9E`
- `not_configured`: `#BDBDBD`

## UX rules
The user must always understand:
- which game/profile is being viewed
- whether paths were detected automatically or manually
- which side is newer
- whether a backup will be created
- what exactly will happen before sync is executed

## UI state
- reflect Syncthing health
- reflect Deck agent availability
- reflect compare status
- disable destructive actions when prerequisites are missing

## Code organization
- UI widgets live in `app/ui/`
- no direct filesystem logic in widgets
- no direct REST logic in widgets
- widgets should call services/core modules only through clean interfaces
