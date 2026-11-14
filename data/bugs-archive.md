---
type: hub
description: Closed bug tickets, fixed or closed without a fix, each keeping its Fix write-up.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🗄️ Bugs archive

*Closed tickets from the [bugs hub](bugs.md): `## Fixed` mirrors `stage: done`,
`## Cancelled` mirrors `stage: cancelled`, newest first. Each keeps its `## Fix`
write-up and its `affected_versions`, so when a bug returns, its ticket is
reopened and its link moves back to the bugs hub.*

``` chart
datasets:
  closed: {query: {filter: {type: bug, stage: {$in: [done, cancelled]}}, project: {stage: 1}}}
hconcat:
  - title: Closed
    data: {name: closed}
    width: 120
    height: 40
    mark: {type: text, fontSize: 28, fontWeight: 600, align: left, baseline: middle, x: 0, y: 20}
    encoding: {text: {aggregate: count}}
  - title: Fixed
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
```

``` chart
title: Fixes per month
height: 180
data: {query: {filter: {type: bug, stage: done}, project: {created: 1, completed: 1, reported_by: 1}}}
transform:
  - calculate: "(toDate(datum.completed) - toDate(datum.created)) / 86400000"
    as: days
encoding:
  x: {field: completed, timeUnit: utcyearmonth, type: ordinal, title: null, axis: {format: "%b %Y", labelAngle: 0}}
layer:
  - mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
    encoding:
      y: {aggregate: count, type: quantitative, title: Fixed, axis: {tickMinStep: 1}}
      color: {field: reported_by, type: nominal, title: Noticed by, scale: {domain: [user, agent], range: ["#0090ff", "#8b8d98"]}, legend: {orient: top}}
  - mark: {type: line, point: true, color: "#30a46c", tooltip: true}
    encoding:
      y: {field: days, aggregate: mean, type: quantitative, title: Mean days to fix}
resolve: {scale: {y: independent}}
```

## Fixed

[Bug: The daily goal ring is clipped in a small
window](bugs/daily-goal-ring-clipped-example.md)

[Bug: The daily goal resets at noon](bugs/daily-goal-resets-at-noon-example.md)

[Bug: The long break comes one session
late](bugs/long-break-count-off-by-one-example.md)

[Bug: The tray icon goes blank after a theme
switch](bugs/tray-icon-blank-after-theme-switch-example.md)

[Bug: Settings reset to defaults after an
update](bugs/settings-reset-after-update-example.md)

[Bug: The start shortcut starts two
sessions](bugs/shortcut-fires-twice-example.md)

[Bug: History shows abandoned sessions as
complete](bugs/history-shows-abandoned-as-complete-example.md)

[Bug: The first break of the day starts
muted](bugs/break-timer-starts-muted-example.md)

[Bug: Pause button stays enabled after a session
completes](bugs/pause-enabled-after-completion-example.md)

[Bug: A running session is lost when the app
quits](bugs/session-lost-on-quit-example.md)

## Cancelled

[Bug: The countdown ends one second
early](bugs/countdown-off-by-one-second-example.md)

[Bug: The timer runs slow on battery](bugs/timer-slow-on-battery-example.md)
