---
type: spec
description: 'Example specification: when Pomodux turns the system do-not-disturb mode on and off.'
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Do not disturb

*Example document — a spec for behaviour that touches the operating system,
where the failure cases matter as much as the normal one. The setup skill
deletes `*-example.md` files after onboarding.*

How Pomodux drives the system do-not-disturb mode when the user has turned the
setting on. With the setting off, Pomodux never touches it.

## Requirements

### Requirement: On for the session

With the setting on, Pomodux SHALL turn on do not disturb when a focus session
starts and turn it off when the session ends, completed or abandoned.

#### Scenario: A session runs to the end

- **GIVEN** the setting is on
- **WHEN** a focus session starts and then completes
- **THEN** do not disturb is on for the session and off once it ends

### Requirement: Off after sleep

If the machine sleeps through the end of a session, Pomodux SHALL turn do not
disturb off when it wakes.

#### Scenario: Waking after the session ended

- **GIVEN** a running session with do not disturb on
- **WHEN** the machine sleeps past the session's end and wakes
- **THEN** do not disturb is off

### Requirement: The user's own setting wins

If do not disturb was already on when the session started, Pomodux SHALL leave
it on when the session ends.

#### Scenario: Already on

- **GIVEN** the user turned do not disturb on before starting a session
- **WHEN** the session ends
- **THEN** do not disturb stays on

*Convention notes: the [do-not-disturb
bug](../bugs/dnd-toggle-ignored-on-wake-example.md) violates "Off after sleep",
and [do-not-disturb sync](../features/do-not-disturb-sync-example.md) is the
feature this spec describes.*
