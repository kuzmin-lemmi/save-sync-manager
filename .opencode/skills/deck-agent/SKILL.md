---
name: deck-agent
description: Rules for the Steam Deck agent and staging-based sync protocol
compatibility: opencode
---

# Deck Agent Rules

## Purpose
The Steam Deck side runs a lightweight agent instead of the full desktop GUI app.

This agent is mandatory for MVP.

## Responsibilities
The agent must:
- monitor the staging protocol directory
- publish heartbeat
- export local Deck saves into staging
- write Deck metadata
- apply incoming PC saves from staging
- confirm completion through marker files
- log all important operations

## Runtime model
- Python script
- installed with `install.sh`
- run as `systemd --user` service

## Protocol files
Inside staging directory:
- `protocol/sync_request.json`
- `protocol/sync_ready.json`
- `protocol/sync_apply.json`
- `protocol/sync_done.json`
- `protocol/heartbeat.json`

## Save directories
Inside staging directory:
- `deck_saves/<appid>/`
- `deck_metadata/<appid>.json`

## Required behavior
### Heartbeat
- update heartbeat regularly
- include timestamp and status

### On sync_request
- validate marker
- resolve the real Deck save path
- export current Deck saves into staging
- write metadata
- write `sync_ready.json`

### On sync_apply
- validate marker
- backup target state if required by architecture
- atomically apply incoming files from staging
- write `sync_done.json`

## Path handling
Support both:
- Proton-based save paths
- native Linux save paths

## Safety
- never blindly delete user files
- prefer temp dir + replace
- detect missing/invalid target path before applying files
- always log failures with enough detail to debug later
