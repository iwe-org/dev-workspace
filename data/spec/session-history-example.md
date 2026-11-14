---
type: spec
description: 'Example specification: what the session log records and how the history view shows completed and abandoned sessions.'
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Session history

*Example document — a spec over stored data: what is recorded, and what the user
is shown. The setup skill deletes `*-example.md` files after onboarding.*

What Pomodux records about each focus session and how the history view presents
it. The [state model](../architecture/state-model-example.md) explains why the
log is append-only; this document says what goes in it.

## Requirements

### Requirement: Every session is logged

Pomodux SHALL append one entry to the session log when a focus session ends,
whether it completed or was abandoned. An entry records the start time, the
planned length, the time actually focused, and whether the session completed.

#### Scenario: Completed session

- **WHEN** a 25-minute session runs to 00:00
- **THEN** the log gains one entry marked completed, with 25 minutes focused

#### Scenario: Abandoned session

- **WHEN** the user abandons a session after 10 minutes
- **THEN** the log gains one entry marked abandoned, with 10 minutes focused

### Requirement: Completed and abandoned are told apart

The history view SHALL list sessions newest first and SHALL mark a completed
session and an abandoned one differently.

#### Scenario: Reading the history

- **GIVEN** one completed and one abandoned session today
- **WHEN** the user opens the history view
- **THEN** only the completed session carries the check mark

### Requirement: Entries are never rewritten

A logged entry SHALL NOT change after it is written. A correction is a new
entry.

*Convention notes: the [abandoned-sessions
bug](../bugs/history-shows-abandoned-as-complete-example.md) violated "Completed
and abandoned are told apart"; the history view is part of [focus
sessions](../features/focus-sessions-example.md), whose plan lists this spec
under its `## Spec changes`.*
