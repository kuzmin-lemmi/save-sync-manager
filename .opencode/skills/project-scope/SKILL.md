---
name: project-scope
description: Architectural boundaries and MVP scope for Save Sync Manager
compatibility: opencode
---

# Project Scope

## Mission
Build an MVP desktop application that syncs game save files between a Windows PC and a Steam Deck.

## Core architecture
This project must follow this architecture:

- Windows desktop app
- Syncthing as a managed external dependency
- shared staging directory synchronized through Syncthing
- lightweight deck agent running on Steam Deck
- marker-file protocol between PC app and Deck agent
- compare before sync
- backup before overwrite

## MVP priorities
1. Core business logic
2. CLI
3. Deck agent
4. Syncthing lifecycle and REST integration
5. GUI

## Explicit non-goals
Do not implement the following in MVP:
- SSH/SFTP transport
- custom cloud backend
- Steam plugin/injection
- auto-hooks into game launch/exit
- non-Steam games
- advanced save merge logic
- “one click magic” without verification

## Safety rules
- Never overwrite save data without backup
- Never simplify sync to “blind folder copy”
- Every sync flow must validate profile, compare state, and record logs
- Prefer atomic file replacement where possible

## Scope discipline
When implementing a module:
- only solve that module’s problem
- do not redesign the whole project
- do not add large new dependencies unless clearly justified
