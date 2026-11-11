---
type: bug
description: The daily goal ring is clipped in the compact window.
stage: done
created: 2026-10-04
completed: 2026-10-10
reported_by: user
affected_versions:
- 0.4.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The daily goal ring is clipped in the compact window

## Symptom

Reported 2026-10-04 on 0.4.0. In the small main window, the right edge of the
goal ring is cut off.

## Expected

The ring fits every window size the app allows.

## Reproduction

1. Resize the main window to its minimum.
2. The ring is clipped on the right.

## Root cause

The ring had a fixed width while the window minimum was lowered in 0.4.0.

## Fix

- **Change:** the ring scales with the window; guarded by
  `src/ui/goal-ring.test.tsx`, listed in the [unreleased
  page](../releases/unreleased.md).
