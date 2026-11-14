---
type: hub
description: Implementation plans, migrations, and design proposals, one document per unit of work.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🗺️ Plans

*Every plan lives in `plans/YYYYMMDD-<slug>.md`. This hub lists the open ones:
`## Active` holds plans without a `stage` or with `stage: approved`, and
`## Deferred` holds plans parked for later. When a plan's `stage` becomes `done`
or `cancelled`, its link moves to the [plans archive](plans-archive.md) in the
same change, so nothing under this hub ever carries a closed stage. The plan
skill files new plans, the ship skill moves them. The chart counts every
`type: plan` document, open and closed.*

``` chart
title: Every plan by stage
height: {step: 24}
data: {query: {filter: {type: plan}, project: {stage: 1}}}
transform:
  - calculate: "datum.stage == null ? 'active' : datum.stage"
    as: state
mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
encoding:
  y: {field: state, type: nominal, title: null, sort: [active, approved, deferred, done, cancelled]}
  x: {aggregate: count, type: quantitative, title: Plans, axis: {tickMinStep: 1}}
  color: {field: state, type: nominal, legend: null, scale: {domain: [active, approved, deferred, done, cancelled], range: ["#8b8d98", "#0090ff", "#f76b15", "#30a46c", "#e5484d"]}}
```

## Active

[1.0](plans/20260625-v1-milestone-example.md)

[Polish](plans/20260901-polish-milestone-example.md)

[Streak widget](plans/20260720-streak-widget-example.md)

[Do-not-disturb sync](plans/20261001-do-not-disturb-sync-example.md)

## Deferred

[Accessibility pass](plans/20260922-accessibility-pass-example.md)
