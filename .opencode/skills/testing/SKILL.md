---
name: testing
description: Testing rules for Save Sync Manager using pytest
compatibility: opencode
---

# Testing Rules

## Framework
- Use `pytest`
- Use `unittest.mock` for mocks
- Do not use `pytest-mock`

## Layout
- Test files must be named `tests/test_<module_name>.py`
- Shared fixtures go into `tests/conftest.py`
- Static test data goes into `tests/fixtures/`

## Naming
- Name tests as `test_<behavior>_<condition>_<result>`
- Example: `test_compare_identical_dirs_returns_identical`

## Minimum expectations
For each important function or module, write at least:
- one happy path test
- one edge case test
- one error/failure test

## Filesystem tests
- Use `tmp_path`
- Create real files and real directories
- Do not mock the filesystem unless absolutely necessary

## External dependencies
- Always mock Syncthing REST API calls
- Always mock external process launch where possible
- Keep business logic tests separate from integration wiring

## Fixtures
Use fixtures for:
- example `libraryfolders.vdf`
- example `appmanifest_<appid>.acf`
- example `game_rules.json`
- temporary save folders
- compare scenarios with different timestamps and file contents

## Assertions
Prefer precise assertions:
- exact statuses
- exact file sets
- exact manifest keys
- exact exception types
- exact relative paths

## Required module coverage
### steam_scanner
- parse valid library file
- parse multiple libraries
- parse appmanifest
- handle missing/broken files

### save_detector
- env expansion
- known path resolution
- Windows to Proton path translation
- fallback heuristic behavior

### compare_engine
- identical
- pc newer
- deck newer
- conflict
- pc only
- deck only
- error state

### backup_manager
- backup creation
- manifest generation
- retention/rotation

### staging_protocol
- marker creation
- marker validation
- stale marker handling
- timeout behavior

### syncthing_client / syncthing_manager
- health check handling
- request formatting
- restart or failure logic

## Command examples
- `pytest tests/ -v`
- `pytest tests/test_compare_engine.py -v`
