---
type: bug
description: History shows abandoned sessions as complete.
stage: done
created: 2026-08-10
completed: 2026-08-14
reported_by: user
affected_versions:
- 0.2.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: History shows abandoned sessions as complete

## Symptom

Reported 2026-08-10 on 0.2.0. In the history view, abandoned sessions carry the
same check mark as completed ones.

## Expected

Abandoned sessions are marked as such, per [Session
history](../features/session-history-example.md).

## Reproduction

1. Start a session and abandon it.
2. Open the history view.
3. The entry shows a check mark.

## Root cause

The row component read `completedAt` instead of the `abandoned` flag.

## Fix

- **Change:** the row reads the flag; guarded by `src/ui/history-row.test.tsx`,
  shipped in [0.3.0](../releases/example-0.3.0.md).
