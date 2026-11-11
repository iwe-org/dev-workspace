---
type: bug
description: A running session is lost when the app quits.
stage: done
created: 2026-07-20
completed: 2026-07-23
reported_by: user
affected_versions:
- 0.1.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: A running session is lost when the app quits

## Symptom

Reported 2026-07-20 on 0.1.0. Quitting Pomodux during a session drops it: the
next launch shows an idle timer and the history has no entry.

## Expected

The [Focus sessions](../features/focus-sessions.example) feature offers to
resume the session on the next launch.

## Reproduction

1. Start a session.
2. Quit from the tray menu.
3. Launch again: the timer is idle.

## Root cause

The quit handler closed the window before the store flushed the in-progress
session.

## Fix

- **Change:** the store flushes on `before-quit`; guarded by
  `src/store/sessions.test.ts`, shipped in [0.2.0](../releases/0.2.0.example).
