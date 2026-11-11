---
type: bug
description: Do not disturb stays on after waking.
created: 2026-10-03
reported_by: user
affected_versions:
- 0.4.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: Do not disturb stays on after waking

## Symptom

Reported 2026-10-03 on 0.4.0. If the machine sleeps during a session, the system
do-not-disturb mode Pomodux turned on stays on after the session ends.

## Expected

Do not disturb ends with the session.

## Reproduction

1. Enable do not disturb during sessions.
2. Sleep the machine mid-session past its end.
3. Wake it: do not disturb is still on.

## Leads

1. Not investigated yet.
