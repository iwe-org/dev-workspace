---
type: hub
description: Open bug tickets, one per reported problem.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🐛 Bugs

*One ticket per problem in `bugs/<slug>.md`, opened before any fix is written:
Symptom, Expected, Reproduction, then the investigation and the Fix. No `stage`
means open and lists the ticket under `## Open`; `stage: deferred` parks it
under `## Deferred`. A ticket closed with `stage: done` (fixed) or `cancelled`
(won't fix) moves to the [bugs archive](bugs-archive.md). A returning bug
reopens its old ticket instead of getting a new one. The charts read every
`type: bug` document, so they stay current without editing this page.*

``` chart
title: Filed and fixed per month
height: 180
data: {query: {filter: {type: bug}, project: {created: 1, completed: 1, stage: 1}}}
transform:
  - fold: [created, completed]
    as: [event, date]
  - filter: "datum.date != null && (datum.event == 'created' || datum.stage == 'done')"
  - calculate: "toDate(datum.date)"
    as: day
  - calculate: "datum.event == 'created' ? 'Filed' : 'Fixed'"
    as: series
mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
encoding:
  x: {field: day, timeUnit: utcyearmonth, type: ordinal, title: null, axis: {format: "%b %Y", labelAngle: 0}}
  xOffset: {field: series, type: nominal, sort: [Filed, Fixed]}
  y: {aggregate: count, type: quantitative, title: Bugs, axis: {tickMinStep: 1}}
  color: {field: series, type: nominal, title: null, scale: {domain: [Filed, Fixed], range: ["#0090ff", "#30a46c"]}, legend: {orient: top}}
```

``` chart
title: Open by who noticed
height: {step: 24}
data: {query: {filter: {type: bug, stage: {$exists: false}}, project: {reported_by: 1}}}
mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
encoding:
  y: {field: reported_by, type: nominal, title: null, sort: [user, agent]}
  x: {aggregate: count, type: quantitative, title: Bugs, axis: {tickMinStep: 1}}
  color: {field: reported_by, type: nominal, legend: null, scale: {domain: [user, agent], range: ["#0090ff", "#8b8d98"]}}
```

``` chart
title: Bugs that came back
height: {step: 24}
data: {query: {filter: {type: bug, affected_versions: {$size: {$gte: 2}}}, project: {title: $title, affected_versions: 1, stage: 1}}}
transform:
  - calculate: "length(datum.affected_versions)"
    as: versions
  - calculate: "join(datum.affected_versions, ', ')"
    as: seen
  - calculate: "datum.stage == 'done' ? 'Fixed' : datum.stage == 'cancelled' ? 'Cancelled' : datum.stage == 'deferred' ? 'Deferred' : 'Open'"
    as: state
mark: {type: bar, cornerRadiusEnd: 3}
encoding:
  y: {field: title, type: nominal, title: null, sort: -x, axis: {labelLimit: 280}}
  x: {field: versions, type: quantitative, title: Versions affected, axis: {tickMinStep: 1}}
  color: {field: state, type: nominal, title: null, scale: {domain: [Open, Deferred, Fixed, Cancelled], range: ["#0090ff", "#f76b15", "#30a46c", "#e5484d"]}, legend: {orient: top}}
  tooltip:
    - {field: title, title: Bug}
    - {field: seen, title: Seen on}
    - {field: state, title: Stage}
```

## Open

[Bug: The long break is skipped after a paused
session](bugs/long-break-skipped-after-pause-example.md)

[Bug: The default shortcut clashes with code
editors](bugs/shortcut-conflicts-with-editor-example.md)

[Bug: The completion chime plays twice](bugs/chime-plays-twice-example.md)

[Bug: Timer drifts after laptop sleep](bugs/timer-drift-after-sleep-example.md)

## Deferred

[Bug: The tray icon is invisible in high
contrast](bugs/high-contrast-tray-icon-invisible-example.md)

[Bug: The screen reader announces every
second](bugs/screen-reader-reads-every-second-example.md)
