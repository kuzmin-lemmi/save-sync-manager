# Save Sync Manager - Agent Guide

## Repository Status
- The repository is still in bootstrap mode: the tracked codebase currently contains only `README.md`.
- There is still no `pyproject.toml`, `requirements*.txt`, `pytest.ini`, `.cursorrules`, `.cursor/rules/`, or `.github/copilot-instructions.md`.
- This file should guide agents while the repository grows from an empty skeleton into the staged MVP described in the spec.

## Product Intent
- Desktop application for syncing game saves between Windows PC and Steam Deck.
- Syncthing is the transport layer, shipped as a managed external dependency.
- The app owns scan, compare, backup, conflict resolution, and safe sync orchestration.
- MVP priority is reliability and recoverability over automation breadth.

## Source Of Truth
- The latest product spec is `tz_save_sync_v2.1.docx` in the repository root.
- When this guide and the spec diverge, prefer the spec and update this file accordingly.
- Follow the staged roadmap: CLI/core first, Deck agent second, GUI MVP third.

## Non-Negotiable Rules
- Use Python 3.10+ syntax and type hints in all production modules.
- Use `pathlib.Path` for filesystem work; never use `os.path`.
- Use `dataclasses` for domain models under `app/models/` and write docstrings in Russian.
- Do not overwrite or delete save data without creating a backup first.
- Prefer explicit, small, low-magic functions over clever abstractions.
- Favor standard library first unless a third-party package clearly improves maintainability.

## Cursor And Copilot Rules
- No Cursor rules were found in `.cursor/rules/` or `.cursorrules`.
- No Copilot instructions were found in `.github/copilot-instructions.md`.
- If any of these files appear later, merge their instructions into this file and remove contradictions.

## Target Project Layout
- `app/ui/` - desktop GUI layer for the PC application.
- `app/core/` - core business logic such as scanning, save detection, compare, sync, backup, and conflict resolution.
- `app/services/` - infrastructure services such as Syncthing REST client, Syncthing process manager, and staging protocol.
- `app/models/` - dataclass-based models like `Game`, `SyncProfile`, `Backup`, and `Conflict`.
- `app/storage/` - JSON-backed persistence for settings, profiles, manifests, and logs.
- `deck_agent/` - Steam Deck agent scripts and installer.
- `data/` - static project data such as `game_rules.json`.
- `config/` - runtime config such as `settings.json` and isolated Syncthing config.
- `tests/` - pytest suite mirroring the source tree.
- `bin/` - bundled Syncthing binaries and license files.

## MVP Stage Priorities
- Stage 1: CLI-oriented core modules, Syncthing lifecycle management, scanning, compare, backup, and one-way sync through staging.
- Stage 1.5: Deck agent, install script, Proton path handling, staging markers, and heartbeat.
- Stage 2: PySide6 GUI wizard, game list, profile configuration, compare screen, and Syncthing diagnostics.
- Do not jump straight to GUI work if core lifecycle, backup, and staging logic are still missing.

## Recommended Bootstrap Commands
- Create venv (PowerShell): `python -m venv .venv`
- Activate venv: `.venv\Scripts\Activate.ps1` or `.venv\Scripts\activate.bat`
- Upgrade packaging tools: `python -m pip install --upgrade pip setuptools wheel`
- Once dependencies exist: `pip install -r requirements.txt`
- Once dev dependencies exist: `pip install -r requirements-dev.txt`
- If the project adopts `pyproject.toml`, prefer `pip install -e .[dev]`.

## Build And Run Commands
- There is no build configuration yet; do not invent packaging commands beyond the spec.
- For early CLI work, prefer `python -m app.cli` once a CLI entrypoint exists.
- For GUI work, prefer `python -m app` or `python -m app.main` once the package is created.
- Packaging should converge on one documented Windows installer path, likely Inno Setup or NSIS.
- The Deck side should expose a documented installer path via `deck_agent/install.sh`.

## Lint, Format, And Type Commands
- No formatter, linter, or type checker is configured yet.
- Recommended baseline once tooling exists: `python -m black .`, `python -m isort .`, `python -m ruff check .`, `python -m mypy app tests`.
- If `ruff` becomes the single formatting tool, prefer `ruff format .` and `ruff check --fix .`.
- Do not assume any of these tools are installed until config files declare them.

## Test Commands
- Run all tests: `pytest`
- Run verbose suite: `pytest -vv`
- Run one file: `pytest tests/path/test_file.py`
- Run one test: `pytest tests/path/test_file.py::test_name`
- Run one parametrized case or exact node id: `pytest tests/path/test_file.py::test_name[param]`
- Run by keyword: `pytest -k "keyword"`
- Stop on first failure: `pytest -x`
- Show locals for one failing test: `pytest tests/path/test_file.py::test_name -vv --maxfail=1 -l`
- Run a narrowed integration subset later, for example: `pytest tests/integration -vv`

## Single-Test Workflow
- Always run the narrowest relevant test first.
- Mirror source paths in tests where practical, for example `app/core/compare_engine.py` -> `tests/core/test_compare_engine.py`.
- For service modules, mirror similarly, for example `app/services/syncthing_client.py` -> `tests/services/test_syncthing_client.py`.
- While iterating, rerun the exact failing node id instead of the whole file.
- After the targeted test passes, expand to the nearest file or related module group.
- Before finishing non-trivial work, run the full suite or clearly state why it cannot run yet.

## Priority Modules To Implement
- `app/core/steam_scanner.py`, `app/core/save_detector.py`, `app/core/compare_engine.py`.
- `app/core/backup_manager.py`, `app/core/sync_manager.py`, `app/core/conflict_resolver.py`.
- `app/services/syncthing_client.py`, `app/services/syncthing_manager.py`, `app/services/staging_protocol.py`.
- `deck_agent/agent.py` and `deck_agent/deck_save_detector.py`.

## Imports
- Group imports as: standard library, third-party, local imports.
- Separate groups with one blank line and prefer absolute imports from the project root package.
- Import only what is used and avoid wildcard imports.
- Extract shared constants and types instead of creating circular imports.
- Prefer `from pathlib import Path` unless multiple `pathlib` symbols are needed.

## Formatting
- Follow PEP 8 unless repository tooling states otherwise.
- Use 4 spaces for indentation and target 88-100 characters if no formatter is configured.
- Keep functions focused and avoid dense branching where a helper improves clarity.
- Use trailing commas in multiline literals and calls when they improve diffs.
- Add comments only when a block is not obvious from names and structure.

## Types And Models
- Add parameter and return types to public functions, methods, and meaningful module constants.
- Prefer precise types like `Path`, `str | None`, `list[Path]`, `dict[str, str]`, and `Sequence[str]`.
- Keep `Any` at external boundaries only.
- Prefer explicit dataclasses over loose dictionaries for internal domain state.
- Use `Enum` for statuses like sync mode, support status, and compare result when helpful.
- Use `TypedDict` only for raw JSON or external API payloads that stay dictionary-shaped.

## Naming
- Modules: lowercase, short, descriptive, underscore-separated.
- Classes: `PascalCase`; functions and methods: `snake_case`; constants: `UPPER_SNAKE_CASE`.
- Private helpers use a leading underscore.
- Test files use `test_*.py` and should mirror the source area they validate.
- Test names should describe behavior, not implementation details.

## Error Handling
- Fail loudly on programmer errors and invalid internal state.
- Raise specific exceptions instead of returning ambiguous sentinel values.
- Wrap low-level failures with domain context when crossing subsystem boundaries.
- Avoid bare `except:`.
- Use retries only for clearly transient operations such as locked files, health checks, or Deck-agent waits.
- Log partial failures without crashing the whole app when one game profile misbehaves.

## Filesystem, JSON, And Atomicity
- Use `Path` end-to-end for paths, joins, checks, and traversal.
- Be explicit about whether paths must exist, may be created, or are optional.
- Use UTF-8 for text I/O unless a different encoding is required.
- Write JSON deterministically where practical, usually with `indent=2`, and use `ensure_ascii=False` when Cyrillic text is possible.
- Validate JSON shape on load instead of assuming keys exist.
- For config, manifest, and save-copy operations, prefer atomic write patterns using temp files or temp directories plus rename.

## Save Safety Rules
- Never replace saves on either side without creating a backup of the target side first.
- Treat backup creation as part of the sync transaction, not as an optional afterthought.
- Preserve enough metadata to restore files later: relative paths, hashes, sizes, and timestamps.
- Keep at least the configured retention window per game; MVP target is 5 backups.
- Staging directories should not intentionally store executables.

## Syncthing Integration Rules
- Treat Syncthing as an external binary, never as embedded or modified source.
- Keep the app-managed Syncthing instance isolated with its own config and port.
- Manage lifecycle through a dedicated service layer, not UI code.
- Use REST API calls for health, devices, folders, and sync status.
- Prefer the separate-instance MVP approach over attaching to a user's existing Syncthing setup.
- Keep the Web UI local-only unless the product explicitly requires something else.

## Steam And Proton Guidance
- Steam library discovery should parse `libraryfolders.vdf` and then `appmanifest_*.acf` files.
- Save detection should use a cascade: rules file, environment expansion, Proton translation, heuristics, then manual override.
- Proton paths should be derived from AppID-based compatdata layout, not guessed from game version names.
- Keep game-specific path rules in `data/game_rules.json` rather than scattering them through code.
- Preserve room for native Linux save paths and future non-Steam launchers without overengineering MVP code.

## Logging And Diagnostics
- Use structured logging, ideally JSON Lines, for sync operations and failures.
- Log important state transitions: compare start, backup created, sync requested, sync delivered, sync applied, conflict detected, retry, and failure.
- Include `appid`, profile id, device side, and path context in logs where relevant.
- Avoid noisy debug logging in hot loops unless gated by log level.

## Testing Expectations
- Add tests for every new module under `app/core/` and `app/services/`.
- Prioritize unit tests for compare logic, save detection, Steam parsing, backup manifests, staging protocol, and Syncthing REST handling.
- Mock Syncthing REST API and subprocess boundaries; do not require a real Syncthing process for unit tests.
- Use `tmp_path` and fixtures for save directories, staging directories, configs, and manifests.
- Add integration tests for compare -> backup -> staging flow once the modules exist.
- Add regression tests for every bug fix, especially around path handling and overwrite safety.

## Agent Working Style
- Inspect existing files before editing and follow local conventions once they exist.
- Make the smallest reasonable change that fully solves the task.
- Keep architecture aligned with the spec's staged rollout instead of inventing unrelated abstractions.
- If you add tooling or entrypoints, update this file so commands stay accurate.
- If a command cannot run because tooling is missing, say so explicitly and give the intended future command.

## When Updating This File
- Replace provisional commands with actual repository commands as soon as config files exist.
- Update the structure section when directories from the spec are created or renamed.
- Merge in any future Cursor or Copilot rules.
- Keep this file concise, practical, and authoritative for coding agents working in the repo.
