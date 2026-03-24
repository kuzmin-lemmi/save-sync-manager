---
name: syncthing-api
description: Syncthing process management and REST API usage for this project
compatibility: opencode
---

# Syncthing Integration Rules

## Core principle
Syncthing is a managed external dependency.
Do not fork it.
Do not embed its source code.
Treat it as a separate binary/process controlled by the app.

## Project model
### On PC
- bundled Syncthing binary
- dedicated config directory for this app
- dedicated local REST port preferred
- avoid interfering with a user’s existing Syncthing instance

### On Steam Deck
- Syncthing installed separately
- Deck agent works alongside it
- app coordinates through staging + protocol files

## HTTP client
- Use `httpx`
- Prefer async client only if there is a clear reason
- Otherwise keep the client synchronous and explicit for MVP

## Base URL
Typical local API base:
`http://localhost:8384/rest/`

## Authentication
- Use `X-API-Key` header
- API key is usually read from Syncthing config
- Never log the raw API key

## Important endpoints
- `GET /rest/system/ping` — health check
- `GET /rest/system/status` — includes local device ID
- `GET /rest/system/connections` — connection state
- `GET /rest/config` — full config
- `GET /rest/config/devices` — devices
- `POST /rest/config/devices` — add/update device
- `GET /rest/config/folders` — folders
- `POST /rest/config/folders` — add/update folder
- `GET /rest/db/status?folder=X` — folder sync status
- `GET /rest/events` — event stream

## Responsibilities split
### syncthing_client.py
- low-level REST calls
- timeout handling
- response parsing
- domain-specific exceptions

### syncthing_manager.py
- process lifecycle
- config/bootstrap
- health checks
- restart logic
- conflict with existing local instance
- staging folder registration

## Implementation rules
- Use explicit request/response handling
- Validate expected keys
- Set timeouts for every request
- Raise meaningful exceptions on API failures
- Keep API wrapper small and testable
