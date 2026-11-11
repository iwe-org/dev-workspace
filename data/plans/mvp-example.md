---
type: plan
description: 'Example milestone plan: the MVP, sequencing the child plans that must ship for it.'
created: 2026-06-25
generated:
  by: human:author
  at: 2026-06-25T00:00:00Z
---

# MVP

*Example document — this shows an aggregator plan: a milestone is a plan whose
children are plans. The setup skill deletes `*-example.md` files after
onboarding.*

Everything Pomodux needs before the first public build: a working timer and the
one piece of retention surface.

## P1 — Retention

[Streak widget](20260720-streak-widget-example.md)

## Sequencing

Focus sessions shipped first, as P0 — the streak widget derives from the session
log it creates. The milestone closes when every child plan carries
`stage: done`.

## Shipped

[Focus sessions](20260701-focus-sessions-example.md)

[Break timers](20260722-break-timers-example.md)

[Session history](20260724-session-history-example.md)

*Convention notes: the child plans are inclusion links, so the milestone's
subtree is queryable
(`iwe retrieve -k data/plans/mvp-example --expand-includes 1`), and every child
carries the tag `milestone-mvp`, which the progress chart in the milestones hub
counts. A child plan that ships moves from its phase to `## Shipped`, so the
phases always show what is left. The milestones hub links this document; the
plans hub lists it under Active like any other plan until its last child ships.*
