---
type: feature
description: 'Example implemented feature: focus sessions, the product''s core loop.'
stage: implemented
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Focus sessions

*Example document — this shows the shape of a feature doc for shipped
functionality. The setup skill deletes `*.example.md` files after onboarding.*

## Purpose

The core loop of Pomodux: a configurable countdown (default 25 minutes) that
records a completed focus session to the local history log.

## Behaviour

Start, pause, resume, and abandon from the main window or the tray. Completion
plays a soft chime and logs the session; abandoned sessions log with an
`abandoned` flag so history stays honest. Timing follows the
[Timer spec](../spec/timer.example) — monotonic clock, sleep-safe.

## Edge cases

- Quitting the app mid-session offers resume-on-next-launch.
- A session shorter than one minute is discarded, not logged.

## Open questions

- Should the default length be onboarding-configurable?
  - Deferred until real usage data exists.

*Convention notes: the feature's `status` chip carries its lifecycle; the story
of how it was built lives in the
[focus sessions plan](../plans/20260701-focus-sessions.example), and the
[0.1.0 release](../releases/0.1.0.example) inclusion-links this doc as shipped
content.*
