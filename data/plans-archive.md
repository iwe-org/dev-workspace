---
type: hub
description: Closed plans — shipped or cancelled — kept reachable from the graph.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🗄️ Plans archive

*Plans whose `stage` is `done` or `cancelled`, moved here from the [plans
hub](plans.md) by the ship skill. Shipped plans sit under the month they shipped
in, newest month first; a plan its milestone also lists under `## Shipped` is
linked from both. Each cancelled plan opens with a line pointing to its
successor or giving the reason it was dropped. Nothing is deleted: the archive
is the record of what was built and what was dropped.*

``` chart
datasets:
  closed: {query: {filter: {type: plan, stage: {$in: [done, cancelled]}}, project: {stage: 1, created: 1, completed: 1}}}
vconcat:
  - hconcat:
      - title: Shipped
        data: {name: closed}
        transform: [{filter: "datum.stage == 'done'"}]
        width: 120
        height: 40
        mark: {type: text, fontSize: 28, fontWeight: 600, align: left, baseline: middle, x: 0, y: 20}
        encoding: {text: {aggregate: count}}
      - title: Cancelled
        data: {name: closed}
        transform: [{filter: "datum.stage == 'cancelled'"}]
        width: 120
        height: 40
        mark: {type: text, fontSize: 28, fontWeight: 600, align: left, baseline: middle, x: 0, y: 20}
        encoding: {text: {aggregate: count}}
  - title: Shipped per month
    height: 160
    data: {name: closed}
    transform:
      - filter: "datum.stage == 'done'"
      - calculate: "(toDate(datum.completed) - toDate(datum.created)) / 86400000"
        as: days
    mark: {type: bar, color: "#30a46c", cornerRadiusEnd: 3}
    encoding:
      x: {field: completed, timeUnit: utcyearmonth, type: ordinal, title: null, axis: {format: "%b %Y", labelAngle: 0}}
      y: {aggregate: count, type: quantitative, title: Plans, axis: {tickMinStep: 1}}
      tooltip:
        - {field: completed, timeUnit: utcyearmonth, type: ordinal, title: Month, format: "%B %Y"}
        - {aggregate: count, title: Plans}
        - {field: days, aggregate: mean, title: Mean days from created, format: ".0f"}
```

## Done

### 2026-10

[Onboarding tour](plans/20260926-onboarding-tour-example.md)

### 2026-09

[Daily goal](plans/20260904-daily-goal-example.md)

[Settings file migration](plans/20260830-settings-file-migration-example.md)

### 2026-08

[Global shortcuts](plans/20260810-global-shortcuts-example.md)

[Long-break scheduling](plans/20260808-long-break-scheduling-example.md)

[Session history](plans/20260724-session-history-example.md)

[Break timers](plans/20260722-break-timers-example.md)

### 2026-07

[Focus sessions](plans/20260701-focus-sessions-example.md)

## Cancelled

[Cloud backup](plans/20260912-cloud-backup-example.md)

[Menu-bar-only timer](plans/20260627-menu-bar-only-timer-example.md)
