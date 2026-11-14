---
type: feature
description: A target number of focus minutes per day, drawn as a ring that fills as sessions complete.
stage: implemented
tests:
- src/store/goal.test.ts
- src/ui/goal-ring.test.tsx
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Daily goal

## Purpose

A daily target of focus minutes, so that "enough for today" is something the
user decides once instead of every evening.

## Behaviour

The target is a setting, off by default. With it on, the main window draws a
ring that fills as today's focused minutes add up, following the [Daily goal
spec](../spec/daily-goal-example.md). Reaching the goal completes the ring and
does nothing else: no notification and no badge, in keeping with [Why
Pomodux](../concept/why-pomodux-example.md). Built by the [daily goal
plan](../plans/20260904-daily-goal-example.md) and shipped in
[0.4.0](../releases/example-0.4.0.md).

## Edge cases

- An abandoned session counts the minutes actually focused.
- The ring scales with the window, down to its minimum size.

## Open questions

- A weekly goal as well?
  - Not until someone asks for it.
