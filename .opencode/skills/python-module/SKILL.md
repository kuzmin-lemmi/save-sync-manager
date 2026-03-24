---
name: python-module
description: Rules for writing Python modules in Save Sync Manager
compatibility: opencode
---

# Python Module Rules

## File structure
- Import order: stdlib -> third-party -> local imports
- Separate import groups with one blank line
- Define module constants near the top using UPPER_CASE
- Create `logger = logging.getLogger(__name__)` in each module

## Typing and signatures
- Use type hints on all function arguments and return values
- Use explicit return types, never leave them implicit
- Prefer small, composable functions over long procedures

## Data models
- Reuse dataclasses from `app/models/models.py`
- Do not create duplicate data models in random modules
- If a new shared model is needed, add it to `app/models/models.py`

## Paths and filesystem
- Use `pathlib.Path` everywhere
- Never use `os.path`
- Use `Path.expanduser()` for `~`
- Use `os.environ.get()` for environment variables, never `os.environ[]`
- Validate paths before destructive actions
- Prefer temporary directory + atomic replace for write operations

## Error handling
- Custom exceptions must live in `app/core/exceptions.py`
- All domain exceptions must inherit from `SaveSyncError`
- Log important operational errors before re-raising or converting them
- Do not swallow exceptions silently

## Logging
- Never use `print()` in application code
- Use structured logging fields where practical
- Include `appid`, `profile_id`, and operation name when available

## Docstrings
- Use Russian docstrings in Google style
- Every public class and public function must have a docstring

## Design principles
- Return structured results, not vague status strings
- Avoid hidden side effects
- Keep modules easy to test in isolation
