---
type: bug
description: The countdown ends one second early.
stage: cancelled
status: deprecated
created: 2026-09-18
reported_by: agent
affected_versions:
- 0.3.1
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The countdown ends one second early

## Symptom

2026-09-18: Found in a flaky test run on 0.3.1: a session ended at 00:01 instead
of 00:00.

## Expected

The countdown shows 00:00 when the completion signal fires.

## Reproduction

1. Not reproduced outside the one test run.

## Fix

- **Change:** none. Not reproducible: the test ran on a loaded CI machine, and
  200 local runs never showed it.
