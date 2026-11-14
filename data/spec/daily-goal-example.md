---
type: spec
description: 'Example specification: what counts toward the daily goal and where a day ends.'
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Daily goal

*Example document — a short spec, written when the plan that built the behaviour
shipped. Low-risk behaviour gets two requirements, not ten. The setup skill
deletes `*-example.md` files after onboarding.*

How Pomodux measures progress toward the daily goal. The goal itself is a
setting; this document fixes what fills it.

## Requirements

### Requirement: Progress counts focused minutes

Progress toward the goal SHALL be the sum of the minutes focused in every
session logged today, completed or abandoned.

#### Scenario: A shortened session

- **GIVEN** a goal of 50 minutes and no session today
- **WHEN** the user abandons a 25-minute session after 10 minutes
- **THEN** progress is 10 of 50 minutes

### Requirement: Day boundary

A session SHALL count toward the local calendar day it ended on, and progress
SHALL start from zero at local midnight.

#### Scenario: Past midnight

- **GIVEN** 40 minutes focused today
- **WHEN** local midnight passes
- **THEN** progress is 0 minutes

*Convention notes: the [daily goal
plan](../plans/20260904-daily-goal-example.md) lists this spec under its
`## Spec changes`; [the goal resetting at
noon](../bugs/daily-goal-resets-at-noon-example.md) violated "Day boundary", and
[daily goal](../features/daily-goal-example.md) is the feature this spec
describes.*
