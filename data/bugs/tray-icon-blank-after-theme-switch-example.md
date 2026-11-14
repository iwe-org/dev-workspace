---
type: bug
description: The tray icon goes blank after a theme switch.
stage: done
created: 2026-08-12
completed: 2026-09-03
reported_by: user
affected_versions:
- 0.2.0
- 0.3.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The tray icon goes blank after a theme switch

## Symptom

Reported 2026-08-12 on 0.2.0. After switching the system between light and dark
mode, the tray icon becomes an empty square.

## Expected

The tray icon follows the system theme without a restart.

## Reproduction

1. Start Pomodux in light mode.
2. Switch the system to dark mode.
3. The tray icon is blank.

## Root cause

The icon was rebuilt from a template image that the theme change had already
released.

## Fix

- **Change:** the icon is rebuilt from the bundled asset on every theme event,
  in both directions.
- **Guard:** `src/ui/tray.test.ts` — "the icon survives light, dark and light
  again". The first fix was tested in one direction only.
- **Shipped:** the first fix in [0.3.0](../releases/example-0.3.0.md); the one
  that held is commit `f4a7c19`, listed under `## Fixed` in
  [0.3.1](../releases/example-0.3.1.md).

## Recurrence

- 2026-08-30, on 0.3.0: the first fix (shipped in 0.3.0) handled the switch to
  dark mode only; switching back still blanked the icon.
