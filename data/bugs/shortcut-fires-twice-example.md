---
type: bug
description: The start shortcut starts two sessions.
stage: done
created: 2026-08-22
completed: 2026-08-25
reported_by: agent
affected_versions:
- unreleased
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The start shortcut starts two sessions

## Symptom

2026-08-22: Found while testing global shortcuts on a build before 0.3.0: one
press of the start shortcut logs two sessions.

## Expected

One press starts one session, per [Global
shortcuts](../features/global-shortcuts-example.md).

## Reproduction

1. Assign a start shortcut.
2. Press it once.
3. History shows two sessions.

## Root cause

The shortcut was registered again every time the settings window opened.

## Fix

- **Change:** registration happens once at launch; guarded by
  `src/shortcuts/register.test.ts`, shipped in
  [0.3.0](../releases/example-0.3.0.md).
