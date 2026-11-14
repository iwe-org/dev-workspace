---
type: feature
description: 'Example cancelled feature: backing up the session log to a cloud drive, dropped in favour of an export.'
stage: cancelled
status: deprecated
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Cloud backup

Cancelled on 2026-09-20: the session log is one small file, the [CSV
export](../backlog/export-history-csv-example.md) covers moving it, and syncing
raises the privacy questions "Local first" in [Why
Pomodux](../concept/why-pomodux-example.md) exists to avoid.

*Example document — this shows a cancelled feature: `stage: cancelled` with OKF
`status: deprecated`, and a first line giving the reason. The document stays in
the features hub, so the next person with the same idea finds the answer. The
setup skill deletes `*-example.md` files after onboarding.*

## Purpose

Back up the session log to the user's cloud drive, so a new machine starts with
the old history.

## Behaviour

As proposed: after every session, copy the log to a folder the user picks inside
a synced drive, and offer to restore from it on a fresh install.

## Edge cases

- Two machines writing to the same folder: the two logs would have to be merged
  entry by entry.

## Open questions

- None left; the reason it was dropped is the first line.
