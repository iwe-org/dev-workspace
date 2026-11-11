---
type: bug
description: The timer runs slow on battery.
stage: cancelled
status: deprecated
created: 2026-08-18
reported_by: user
affected_versions:
- 0.2.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The timer runs slow on battery

## Symptom

Reported 2026-08-18 on 0.2.0. On battery, the countdown seems to lag behind a
stopwatch by a few seconds per session.

## Expected

The countdown matches wall time to the second.

## Reproduction

1. Unplug the laptop.
2. Run a 25-minute session next to a stopwatch.

## Fix

- **Change:** none. By design: the system throttles timers on battery, and the
  countdown re-syncs from the monotonic clock on every tick, so the end time is
  exact. A stopwatch comparison showed under a second of drift.
