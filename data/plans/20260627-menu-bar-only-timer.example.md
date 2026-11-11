---
type: plan
description: 'Example cancelled plan: a menu-bar-only timer, superseded by the focus sessions plan before any code landed.'
stage: cancelled
status: deprecated
created: 2026-06-27
generated:
  by: human:author
  at: 2026-07-01T00:00:00Z
---

# Menu-bar-only timer

Replaced on 2026-07-01 by [Focus sessions](20260701-focus-sessions.example),
which puts the countdown in the main window and mirrors it in the tray.

*Example document — this shows a cancelled plan: `stage: cancelled` (with OKF
`status: deprecated`), a first line pointing to its successor, and its link
under `## Cancelled` in the plans archive. The setup skill deletes
`*.example.md` files after onboarding.*

## Context

The first idea for Pomodux was a timer that lives only in the menu bar, with no
main window at all.

## Approach

A tray icon whose label is the countdown, and a dropdown with start, pause and
abandon.

## Verification

- Manual: a session can be started, paused and finished from the tray alone.

## Out of scope

- Session history, which needs a window to browse it.

*Convention notes: the focus sessions plan links back here, and both edits
landed together. Dropped work gets `stage: cancelled` right away — left without
a stage, this plan would still sit under Active and look like pending work.*
