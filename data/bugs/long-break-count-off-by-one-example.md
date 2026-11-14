---
type: bug
description: The long break comes one session late.
stage: done
created: 2026-09-10
completed: 2026-09-16
reported_by: agent
affected_versions:
- 0.3.1
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The long break comes one session late

## Symptom

2026-09-10: Found during a review of the scheduler on 0.3.1: with a long break
every four sessions, it arrives after the fifth.

## Expected

The long break follows the configured number of sessions, per "Long-break
interval" in the [Breaks spec](../spec/breaks-example.md).

## Reproduction

1. Set a long break every 4 sessions.
2. Complete four sessions.
3. A short break starts instead.

## Root cause

The counter was incremented after the check instead of before.

## Fix

- **Change:** the session counter is incremented before the interval check, in
  `src/timer/schedule.ts`.
- **Guard:** `src/timer/schedule.test.ts` — "the long break follows the fourth
  session".
- **Shipped:** commit `b3187ce`, listed under `## Fixed` in
  [0.4.0](../releases/example-0.4.0.md).
