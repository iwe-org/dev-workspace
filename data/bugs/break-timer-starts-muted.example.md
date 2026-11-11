---
type: bug
description: The first break of the day starts muted.
stage: done
created: 2026-08-03
completed: 2026-08-06
reported_by: agent
affected_versions:
- unreleased
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The first break of the day starts muted

## Symptom

2026-08-03: Found while writing break-timer tests on a build before 0.2.0: the
first break after launch plays no chime.

## Expected

Every break starts with the chime, as [Break
timers](../features/break-timers.example) describes.

## Reproduction

1. Launch the app.
2. Finish one focus session.
3. The break starts silently.

## Root cause

The audio element was created lazily on the first chime and missed it.

## Fix

- **Change:** the chime is preloaded at launch; guarded by
  `src/audio/chime.test.ts`, shipped in [0.2.0](../releases/0.2.0.example).
