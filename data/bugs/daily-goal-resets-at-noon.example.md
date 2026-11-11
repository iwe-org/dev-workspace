---
type: bug
description: The daily goal resets at noon.
stage: done
created: 2026-09-21
completed: 2026-09-23
reported_by: agent
affected_versions:
- unreleased
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The daily goal resets at noon

## Symptom

2026-09-21: Found while testing the daily goal on a build before 0.4.0: progress
drops to zero at 12:00.

## Expected

Progress resets at local midnight, the rule in [streak
days](../learnings/streak-days-use-local-midnight.example).

## Reproduction

1. Complete a session in the morning.
2. Wait until noon.
3. The goal ring is empty.

## Root cause

The day boundary was computed from a 12-hour clock value.

## Fix

- **Change:** days come from the local date; guarded by
  `src/store/goal.test.ts`, shipped in [0.4.0](../releases/0.4.0.example).
