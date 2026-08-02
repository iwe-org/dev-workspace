---
type: architecture
description: 'Example architecture note: how session state is modelled and why the rejected alternatives lost.'
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
sources:
- id: store
  resource: src/store
  title: the store module the model describes
  author: human:author
  last_modified: 2026-07-25
---

# State model

*Example document — this shows the shape of an architecture note: the design as
built, the decision that produced it, and the alternatives rejected. The setup
skill deletes `*.example.md` files after onboarding.*

Pomodux keeps all mutable state in a single store (`src/store/sessions.ts`)
updated through pure reducer functions; the UI subscribes and renders, never
mutates. Completed sessions append to a JSON-lines log in the user data
directory — the log is the source of truth, and aggregates (daily totals,
streaks) are derived at read time.

## Decision: append-only session log

An append-only log was chosen over a mutable database because sessions are
immutable facts, crash recovery reduces to replaying the file, and sync between
machines becomes a merge of two logs.

Rejected alternatives:

- **SQLite** — transactional power we don't need; adds a native dependency to an
  otherwise pure-TypeScript build.
- **Mutable JSON snapshot** — a crash during write can lose the whole history,
  not just the last entry.

*Convention notes: like specs, architecture notes carry no frontmatter. Record
the decision when it's made, with the rejected options — the
[streak widget plan](../plans/20260720-streak-widget.example) builds directly on
the derived-aggregates rule stated here.*
