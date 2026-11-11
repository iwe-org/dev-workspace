---
type: hub
description: Feature design documents, each carrying its lifecycle stage.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# ✨ Features

*One document per feature in `features/<slug>.md`, from first proposal to
deprecation — the `stage` frontmatter (`proposed` → `accepted` → `implemented`,
terminal `deprecated`/`cancelled`) carries the lifecycle, so the list stays
flat. An implemented feature names its guard tests in `tests`; the tile counts
them. Group with `##` headings once it outgrows a single list.*

``` chart
data: {query: {filter: {type: feature}, project: {stage: 1, tests: 1}}}
vconcat:
  - title: Guard tests
    transform:
      - calculate: "isArray(datum.tests) ? length(datum.tests) : 0"
        as: guards
    width: 120
    height: 40
    mark: {type: text, fontSize: 28, fontWeight: 600, align: left, baseline: middle, x: 0, y: 20}
    encoding: {text: {field: guards, aggregate: sum}}
  - title: Features by stage
    height: {step: 24}
    mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
    encoding:
      y: {field: stage, type: nominal, title: null, sort: [proposed, accepted, implemented, deprecated, cancelled]}
      x: {aggregate: count, type: quantitative, title: Features, axis: {tickMinStep: 1}}
      color: {field: stage, type: nominal, legend: null, scale: {domain: [proposed, accepted, implemented, deprecated, cancelled], range: ["#ffc53d", "#0090ff", "#30a46c", "#f76b15", "#e5484d"]}}
```

[Break timers](features/break-timers-example.md)

[Daily goal](features/daily-goal-example.md)

[Focus sessions](features/focus-sessions-example.md)

[Global shortcuts](features/global-shortcuts-example.md)

[Long-break scheduling](features/long-break-scheduling-example.md)

[Onboarding tour](features/onboarding-tour-example.md)

[Session history](features/session-history-example.md)

[Do-not-disturb sync](features/do-not-disturb-sync-example.md)

[Task labels](features/task-labels-example.md)

[Custom sounds](features/custom-sounds-example.md)

[Streak widget](features/streak-widget-example.md)

[Weekly report](features/weekly-report-example.md)

[Compact mode](features/compact-mode-example.md)

[Cloud backup](features/cloud-backup-example.md)
