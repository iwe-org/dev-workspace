---
type: bug
description: Settings reset to defaults after an update.
stage: done
created: 2026-08-29
completed: 2026-09-02
reported_by: user
affected_versions:
- 0.3.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: Settings reset to defaults after an update

## Symptom

Reported 2026-08-29 on 0.3.0. After updating to 0.3.0, session length and
shortcuts are back to their defaults.

## Expected

Settings survive an update.

## Reproduction

1. Change the session length on 0.2.0.
2. Update to 0.3.0.
3. The length is back to 25 minutes.

## Root cause

0.3.0 renamed two settings keys and read the new names only.

## Fix

- **Change:** the [settings file
  migration](../plans/20260830-settings-file-migration-example.md) plan; guarded
  by `src/settings/migrate.test.ts`, shipped in
  [0.3.1](../releases/example-0.3.1.md).
