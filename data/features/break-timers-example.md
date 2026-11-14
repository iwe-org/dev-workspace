---
type: feature
description: A break after every focus session, short by default and longer after every fourth, each with its own countdown and chime.
stage: implemented
tests:
- src/timer/break.test.ts
- src/timer/schedule.test.ts
- src/audio/chime.test.ts
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Break timers

## Purpose

A break after every focus session, so that stopping is part of the loop instead
of a decision the user makes each time: a short break by default, a longer one
after every fourth session.

## Behaviour

When a session completes, the break countdown starts by itself and ends with its
own chime. The two break lengths (5 and 15 minutes by default) and the
long-break interval are settings, and a break can be skipped. Which break starts
when is fixed by the [Breaks spec](../spec/breaks-example.md). Shipped in
[0.2.0](../releases/example-0.2.0.md).

## Edge cases

- An abandoned session is not followed by a break and does not count toward the
  long break.
- Quitting during a break drops the break; nothing is logged for it.

## Open questions

- Should a break start by itself or wait for a click?
  - It starts by itself; revisit if users report missed breaks.
