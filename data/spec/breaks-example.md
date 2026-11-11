---
type: spec
description: 'Example specification: when short and long breaks start, and which sessions count toward a long break.'
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Breaks

*Example document — a second spec in the same shape as the
[timer](timer-example.md): requirements a reviewer can check against the code.
The setup skill deletes `*-example.md` files after onboarding.*

How Pomodux schedules the breaks between focus sessions. The break length and
the long-break interval are settings; this document fixes what they mean.

## Requirements

### Requirement: Break after every session

A completed focus session SHALL be followed by a break: a short break, or a long
break when the long-break interval is reached.

#### Scenario: Short break

- **GIVEN** a long break every 4 sessions and 1 session completed today
- **WHEN** the second session completes
- **THEN** a short break of the configured length starts

### Requirement: Long-break interval

A long break SHALL start after every N completed sessions, where N is the
configured interval (4 by default). Every completed session counts toward N,
including one that was paused and resumed. An abandoned session does not.

#### Scenario: Paused sessions count

- **GIVEN** a long break every 2 sessions
- **WHEN** the user pauses and resumes the first session, then completes both
- **THEN** a long break starts after the second session

#### Scenario: Abandoned sessions do not count

- **GIVEN** a long break every 2 sessions
- **WHEN** the user abandons one session and completes two others
- **THEN** the long break starts after the second completed session

### Requirement: Skipping a break

The user SHALL be able to skip a break. A skipped break does not reset the
long-break count.

#### Scenario: Skipping a short break

- **GIVEN** a short break has started
- **WHEN** the user skips it
- **THEN** the next focus session can start at once, and the session count is
  unchanged

*Convention notes: the [long-break
bug](../bugs/long-break-skipped-after-pause-example.md) violates "Long-break
interval", and [long-break
scheduling](../features/long-break-scheduling-example.md) is the feature this
spec describes.*
