---
type: plan
description: 'Example milestone plan: 1.0, sequencing the child plans that must ship before a release is called 1.0.'
created: 2026-06-25
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# 1.0

*Example document — this shows an aggregator plan: a milestone is a plan whose
children are plans. The setup skill deletes `*-example.md` files after
onboarding.*

Everything Pomodux needs before a release is called 1.0: a working timer, which
every 0.x release has carried since 0.1.0, and the one piece of retention
surface.

## P1 — Retention

[Streak widget](20260720-streak-widget-example.md)

## Sequencing

Focus sessions shipped first, in 0.1.0 — the streak widget derives from the
session log it creates. The 0.x releases since then came out of the
[Polish](20260901-polish-milestone-example.md) milestone and bug fixes, and none
of them closes this one: the milestone closes when every child plan carries
`stage: done`.

## Shipped

[Focus sessions](20260701-focus-sessions-example.md)

*Convention notes: the child plans are inclusion links, so the milestone's
subtree is queryable
(`iwe retrieve -k data/plans/20260625-v1-milestone-example --expand-includes 1`),
and every child carries the tag `milestone-v1`, which the progress chart in the
milestones hub counts. A child plan that ships moves from its phase to
`## Shipped`, so the phases always show what is left. The milestones hub links
this document; the plans hub lists it under Active like any other plan until its
last child ships.*
