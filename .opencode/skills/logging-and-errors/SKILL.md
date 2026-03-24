---
name: logging-and-errors
description: Structured logging and exception handling rules for Save Sync Manager
compatibility: opencode
---

# Logging and Error Handling

## Logging
- Use `logging`, never `print`
- Prefer machine-readable structured messages
- Include context when possible:
  - appid
  - profile_id
  - operation
  - source_device
  - target_device

## Log levels
- DEBUG for low-level diagnostics
- INFO for normal operational milestones
- WARNING for recoverable issues
- ERROR for failed operations
- CRITICAL only for unrecoverable startup/runtime failures

## Exception hierarchy
Create a base exception:
- `SaveSyncError`

Derived exceptions may include:
- `ConfigError`
- `SteamScanError`
- `SaveDetectionError`
- `CompareError`
- `BackupError`
- `SyncProtocolError`
- `SyncthingError`
- `DeckAgentError`

## Rules
- Convert low-level exceptions into domain exceptions when crossing module boundaries
- Do not over-wrap exceptions unnecessarily
- Log once at the right boundary; avoid duplicate noisy logging
- Never hide destructive errors during sync/backup flows
