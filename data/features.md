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
hconcat:
  - title: Features by stage
    height: {step: 24}
    mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
    encoding:
      y: {field: stage, type: nominal, title: null, sort: [proposed, accepted, implemented, deprecated, cancelled]}
      x: {aggregate: count, type: quantitative, title: Features, axis: {tickMinStep: 1}}
      color: {field: stage, type: nominal, legend: null, scale: {domain: [proposed, accepted, implemented, deprecated, cancelled], range: ["#ffc53d", "#0090ff", "#30a46c", "#f76b15", "#e5484d"]}}
  - title: Guard tests
    transform:
      - calculate: "isArray(datum.tests) ? length(datum.tests) : 0"
        as: guards
    mark: {type: text, fontSize: 28, fontWeight: 600}
    encoding: {text: {field: guards, aggregate: sum}}
```

[Break timers](features/break-timers.example)

[Daily goal](features/daily-goal.example)

[Focus sessions](features/focus-sessions.example)

[Global shortcuts](features/global-shortcuts.example)

[Long-break scheduling](features/long-break-scheduling.example)

[Onboarding tour](features/onboarding-tour.example)

[Session history](features/session-history.example)

[Do-not-disturb sync](features/do-not-disturb-sync.example)

[Task labels](features/task-labels.example)

[Custom sounds](features/custom-sounds.example)

[Streak widget](features/streak-widget.example)

[Weekly report](features/weekly-report.example)

[Compact mode](features/compact-mode.example)

[Cloud backup](features/cloud-backup.example)
