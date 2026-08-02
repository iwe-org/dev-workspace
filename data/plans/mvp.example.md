---
created: 2026-06-25
---

# MVP

*Example document — this shows an aggregator plan: a milestone is a plan whose
children are plans. The setup skill deletes `*.example.md` files after
onboarding.*

Everything Pomodux needs before the first public build: a working timer and the
one piece of retention surface.

## P0 — Core loop

[Focus sessions](20260701-focus-sessions.example)

## P1 — Retention

[Streak widget](20260720-streak-widget.example)

## Sequencing

Focus sessions ships first — the streak widget derives from the session log it
creates. The milestone closes when both child plans carry `status: done`.

*Convention notes: the child plans are inclusion links, so the milestone's
subtree is queryable
(`iwe retrieve -k data/plans/mvp.example --expand-includes 1`). The milestones
hub links this document; the plans hub lists it under Active like any other
plan.*
