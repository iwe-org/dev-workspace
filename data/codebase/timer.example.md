---
type: codebase
description: 'Example codebase map: the timer module and its tick and drift handling.'
source: src/timer
commit: 3f1a9c2
stale_after: 2026-11-01
sources:
- id: code
  resource: src/timer
  title: the code this map describes, read at commit 3f1a9c2
  author: human:author
  last_modified: 2026-07-25
verified:
  by: human:author
  at: 2026-07-25T00:00:00Z
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Timer engine

*Example document — this shows the shape of a codebase-map component doc:
derived from reading the code, stamped with what it describes and the revision
it was read at. The setup skill deletes `*.example.md` files after onboarding.*

The countdown state machine behind focus sessions: pure, UI-free, driven by a
monotonic clock. The smallest component in Pomodux and deliberately so — every
timing decision lives here and nowhere else.

## Public surface

- `createEngine(config)` — `src/timer/engine.ts:12` — construct with session
  length and run-through-sleep policy
- `start() / pause() / resume() / abandon()` — `src/timer/engine.ts:27`
- `onTransition(cb)` — `src/timer/engine.ts:63` — the only way state leaves this
  component

## How it works

States: idle → running → paused → completed | abandoned. Elapsed time is
accumulated from `performance.now()` deltas per run segment, so wall-clock
changes never corrupt a session (the requirement behind this is in the
[Timer spec](../spec/timer.example)). A wake-event handler reconciles once after
system sleep.

## Depends on

Nothing internal — transitions are consumed by the
[session store](store.example); the dependency points the other way. No UI
imports, ever.

## Invariants & gotchas

- Never read `Date.now()` for elapsed time — the open
  [timer drift bug](../bugs/timer-drift-after-sleep.example) is what happens
  when this slips.
- A session under one minute emits `abandoned`, not `completed`.

## Key references

Verified anchor points (line numbers as of 2026-07-25):

- `src/timer/engine.ts:42` — `tick()`
- `src/timer/engine.ts:88` — wake-event reconciliation

Convention notes: the key is canonical — code at `src/timer` maps to
`data/codebase/timer` (wrapper segments like `src/` are elided), so an agent
holding a code path computes the doc key without searching. Together, `source`,
`commit`, and `verified` make staleness queryable — the refresh pass runs
`git log <commit>..HEAD -- <source>` and re-reads only dirty components.
`## Depends on` is written; "used by" is a query:
`iwe find --references data/codebase/timer.example`.
