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

The long break follows the configured number of sessions, per [Long-break
scheduling](../features/long-break-scheduling.example).

## Reproduction

1. Set a long break every 4 sessions.
2. Complete four sessions.
3. A short break starts instead.

## Root cause

The counter was incremented after the check instead of before.

## Fix

- **Change:** the counter moves before the check; guarded by
  `src/timer/schedule.test.ts`, shipped in [0.4.0](../releases/0.4.0.example).
