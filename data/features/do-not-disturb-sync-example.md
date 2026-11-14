---
type: feature
description: Turn on the system's do-not-disturb mode for the length of a focus session.
stage: accepted
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Do-not-disturb sync

## Purpose

Turn on the system's do-not-disturb mode for the length of a focus session, so
the session is protected from other applications the way [Why
Pomodux](../concept/why-pomodux-example.md) protects it from Pomodux itself.

## Behaviour

A setting, off by default. With it on, do not disturb goes on when a session
starts and off when it ends, completed or abandoned. If it was already on when
the session started, it is left on. The [do-not-disturb sync
plan](../plans/20261001-do-not-disturb-sync-example.md) is approved and not
started.

## Edge cases

- The machine sleeps past the end of the session: do not disturb goes off on
  wake.
- The system asks for permission on first use; Pomodux asks when the setting is
  turned on, [never during a
  session](../learnings/dnd-needs-permission-first-example.md).

## Open questions

- Should breaks be covered too?
  - No: a break is when messages are welcome.
