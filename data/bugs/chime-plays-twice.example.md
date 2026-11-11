---
type: bug
description: The completion chime plays twice.
created: 2026-07-30
reported_by: user
affected_versions:
- 0.1.0
- 0.2.0
- 0.4.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The completion chime plays twice

## Symptom

Reported 2026-07-30 on 0.1.0. Seen again on 0.4.0: when a session completes
while the history view is open, the chime plays twice.

## Expected

One chime per completed session.

## Reproduction

1. Open the history view.
2. Let a session complete.
3. Two chimes play.

## Root cause

Every window subscribed to the completion event and played the chime itself.

## Fix

- **Change:** the main window plays the chime once per session; shipped in
  [0.2.0](../releases/0.2.0.example).

## Recurrence

- 2026-10-06, on 0.4.0: the 0.2.0 fix deduplicated chimes from the main window
  only; the history view subscribes on its own.
