---
type: bug
description: The daily goal ring is clipped in a small window.
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

# Bug: The daily goal ring is clipped in a small window

## Symptom

Reported 2026-10-04 on 0.4.0. In the small main window, the right edge of the
goal ring is cut off.

## Expected

The ring fits every window size the app allows, an edge case the [Daily
goal](../features/daily-goal-example.md) feature names.

## Reproduction

1. Resize the main window to its minimum.
2. The ring is clipped on the right.

## Root cause

The ring had a fixed width while the window minimum was lowered in 0.4.0.

## Fix

- **Change:** the ring's size derives from the window width, in
  `src/ui/goal-ring.tsx`.
- **Guard:** `src/ui/goal-ring.test.tsx` — "the ring fits the minimum window".
- **Shipped:** commit `e90c4b2`, listed under `## Fixed` on the [unreleased
  page](../releases/unreleased.md).
