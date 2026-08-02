---
type: codebase
description: 'Example codebase map: the streak slice of the store and how it is computed.'
source: src/store/streak.ts
commit: 3f1a9c2
stale_after: 2026-11-01
sources:
- id: code
  resource: src/store/streak.ts
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

# Streak calculator

*Example document — a leaf component nested under its parent: the canonical key
mirrors the code path (`src/store/streak.ts` → `data/codebase/store/streak`),
and the only inclusion link to it comes from the parent's `## Contains`. The
setup skill deletes `*.example.md` files after onboarding.*

Derives the consecutive-days streak from the session log at read time — nothing
is stored, so the streak can never disagree with history.

## Public surface

- `computeStreak(log, timezone)` — `src/store/streak.ts:8` — the only export

## How it works

A single fold over the log: group completed sessions by local day, count
backwards from today until the first gap.

## Depends on

Only the session-log format owned by its parent `store` component (the tree edge
above encodes the containment — a doc never inline-links its own parent); no
engine or UI imports.

## Invariants & gotchas

- Day boundaries derive from local midnight at completion time — recomputing in
  another timezone can change the streak, by design.

## Key references

Verified anchor points (line numbers as of 2026-07-25):

- `src/store/streak.ts:8` — `computeStreak()`
- `src/store/streak.ts:23` — gap detection

*Convention notes: a component too small for its own doc stays a section of its
parent; this one earns a doc because agents change it independently of the
store. "Used by" is the query
`iwe find --references data/codebase/store/streak.example`.*
