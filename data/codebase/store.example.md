---
source: src/store
commit: 3f1a9c2
verified: 2026-07-25
---

# Session store

*Example document — a container component: its `## Contains` inclusion links put
child components in the tree, so `iwe tree -k data/codebase` renders the code's
structure. The setup skill deletes `*.example.md` files after onboarding.*

The single mutable store: reduces engine transitions into app state and persists
completed sessions to the append-only log. All aggregates (history views,
streaks) are derived at read time — the design and its rejected alternatives are
recorded in the [state model](../architecture/state-model.example).

## Contains

[Streak calculator](store/streak.example)

## Public surface

- `appendSession(session)` — `src/store/sessions.ts:15` — the log writer
- `subscribe(selector, cb)` — `src/store/sessions.ts:44` — UI read path

## How it works

Pure reducers over engine transitions; the JSON-lines log in the user data
directory is the source of truth, replayed on startup for crash recovery.

## Depends on

Consumes transitions from the [timer engine](timer.example). Writes through the
platform file API only — no other component touches the log file.

## Invariants & gotchas

- The log is append-only; corrections are new entries, never edits.

## Key references

Verified anchor points (line numbers as of 2026-07-25):

- `src/store/sessions.ts:15` — `appendSession()`
- `src/store/reducers.ts:21` — transition reducer table

*Convention notes: the map doc describes what the code does; the
[architecture note](../architecture/state-model.example) records why — a map
refresh may rewrite this doc freely, but never touches the decision record.
Containment is written one way (this doc lists its children); "part of" is the
backlink.*
