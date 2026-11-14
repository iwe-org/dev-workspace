---
type: plan
description: 'Example shipped plan: the daily goal, derived from the session log and drawn as a ring.'
stage: done
created: 2026-09-04
completed: 2026-09-24
tags:
- milestone-polish
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Daily goal

## Context

Builds the [Daily goal](../features/daily-goal-example.md) feature, the first
child of the [Polish](20260901-polish-milestone-example.md) milestone. Like a
streak, today's progress is an aggregate derived from the session log at read
time, the rule the [state model](../architecture/state-model-example.md) sets.

## Approach

A pure `focusedMinutes(log, day)` function over the session log, compared with a
`goalMinutes` setting and drawn as a ring in the main window. Days follow the
[local-day rule](../learnings/streak-days-use-local-midnight-example.md).
Rejected: a stored running total, which is a second source of truth beside the
log and needs a job that resets it at midnight.

## Implementation Steps

### Task 1: Goal derivation

**Files:** Create: `src/store/goal.ts`, `src/store/goal.test.ts`

- [x] `focusedMinutes` over the entries of one local day
- [x] An abandoned session counts the minutes actually focused
- [x] Day-boundary tests on the fake clock

### Task 2: Goal ring

**Files:** Create: `src/ui/goal-ring.tsx`, `src/ui/goal-ring.test.tsx`; Modify:
`src/ui/timer-view.tsx`, `src/ui/settings.tsx`

- [x] The ring fills with the share of the goal reached, and caps at full
- [x] A goal setting, off by default; no ring while it is off

## Spec changes

[Daily goal](../spec/daily-goal-example.md) — created when this plan shipped:
what counts toward the goal and where a day ends.

## Verification

- `npm test -- src/store/goal src/ui/goal-ring` — green.
- Manual: with a 50-minute goal, one completed 25-minute session fills half the
  ring, and the ring is empty again after local midnight.

## Out of scope

- Weekly or monthly goals.
- Any notification when the goal is reached: nothing interrupts a session.

## Key references

Verified anchor points (line numbers as of 2026-09-04):

- `src/store/sessions.ts:15` — `appendSession()`, the log the goal reads
- `src/ui/timer-view.tsx:22` — `TimerView`, where the ring mounts
