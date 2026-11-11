---
type: bug
description: 'Example open bug: the timer drifts after the machine sleeps, with repro and investigation notes.'
created: 2026-07-25
reported_by: user
affected_versions:
- 0.1.0
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Bug: Timer drifts after laptop sleep

*Example document — this shows the shape of an open bug ticket: no `stage`
frontmatter, listed under `## Open` in the bugs hub. The setup skill deletes
`*-example.md` files after onboarding.*

## Symptom

Reported 2026-07-25 on 0.1.0. After the laptop sleeps during a focus session,
the timer sometimes shows more remaining time than before sleep, or jumps
straight to negative values.

## Expected

The countdown resumes from the time actually left, as the "Wall-clock
independence" requirement in the [Timer spec](../spec/timer-example.md) says.

## Reproduction

1. Start a 25-minute session, wait until ~10:00 remaining.
2. Close the lid for 5+ minutes, reopen.
3. Timer shows 10:04, 10:31, or `-42:17` depending on sleep length.

## Leads

1. Most likely: the tick handler recomputes remaining time from `Date.now()`
   deltas, so the wall clock leaks into elapsed time across sleep. Cheapest
   check: log both `Date.now()` and `performance.now()` around one sleep.
2. If so, compute elapsed time from the monotonic clock and reconcile once on
   the wake event instead of every tick.

## Key references

Verified anchor points (line numbers as of 2026-07-25):

- `src/timer/engine.ts:42` — `tick()`, the wall-clock delta
- `src/timer/engine.ts:88` — wake-event handler, currently a no-op

*Convention notes: `created`, `reported_by` and `affected_versions` are
required. `reported_by: user` because a person noticed it; an agent typing the
ticket does not change that. When the fix ships, the ticket gets `stage: done`,
`completed`, and a `## Fix` section, and its link moves to `## Fixed` in the
bugs archive. Should it return later, reopen this same ticket: append the new
version to `affected_versions`, drop `stage`, and add a dated `## Recurrence`
entry.*
