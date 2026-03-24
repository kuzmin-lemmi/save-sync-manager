---
name: spec-writing
description: Rules for writing module specifications before implementation
compatibility: opencode
---

# Spec Writing Rules

## Goal
Before implementing an important module, first write a short module spec.

## Required sections
Each module spec should include:
- purpose
- inputs
- outputs
- dependencies
- error cases
- test cases
- non-goals

## Scope
One spec per module.
Do not write giant all-project specs for small tasks.

## Example modules
- steam_scanner
- save_detector
- compare_engine
- backup_manager
- staging_protocol
- syncthing_client
- syncthing_manager
- deck_agent

## Acceptance mindset
Every spec should answer:
- what problem this module solves
- what it must not do
- how we know it works
