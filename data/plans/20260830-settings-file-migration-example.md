---
type: plan
description: Plan for settings file migration.
stage: done
created: 2026-08-30
completed: 2026-09-02
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Settings file migration

## Context

Fixes [settings reset after an
update](../bugs/settings-reset-after-update-example.md): 0.3.0 renamed settings
keys without migrating them.

## Implementation Steps

### Task 1: Settings file migration

- [x] Implement and test

## Verification

- `npm test` green, plus a manual pass over the feature's behaviour.
