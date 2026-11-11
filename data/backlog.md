---
type: hub
description: Prioritized single-topic work items waiting to become a plan or a quick fix.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 📋 Backlog

*Prioritized work waiting to become plans, one task per document in
`backlog/<slug>.md`. A finished task gets `stage: done` + `completed` and its
link moves to `## Done`; a task parked for later gets `stage: deferred` and
moves to `## Deferred`, and a dropped one gets `stage: cancelled` and moves to
`## Cancelled`. When a task grows past a single sitting, promote it: write a
plan and link the plan instead.*

``` chart
data: {query: {filter: {type: task}, project: {stage: 1, priority: 1, created: 1}}}
hconcat:
  - title: Tasks by stage and priority
    height: {step: 24}
    mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
    encoding:
      y: {field: stage, type: nominal, title: null, sort: [planned, deferred, done, cancelled]}
      x: {aggregate: count, type: quantitative, title: Tasks, axis: {tickMinStep: 1}}
      color: {field: priority, type: nominal, title: null, sort: [high, medium, low], scale: {domain: [high, medium, low], range: ["#e5484d", "#f76b15", "#8b8d98"]}, legend: {orient: bottom, direction: horizontal}}
      order: {field: priority, sort: ascending}
  - title: Added per month
    height: 120
    mark: {type: bar, color: "#0090ff", cornerRadiusEnd: 3}
    encoding:
      x: {field: created, timeUnit: utcyearmonth, type: ordinal, title: null, axis: {format: "%b", labelAngle: 0}}
      y: {aggregate: count, type: quantitative, title: Tasks, axis: {tickMinStep: 1}}
```

## High

[Export the session history as CSV](backlog/export-history-csv-example.md)

[Fill in the product doc](backlog/fill-product-doc.md)

[Plan the first feature](backlog/plan-first-feature.md)

## Medium

[Sharpen the tray icon on high-density
displays](backlog/retina-tray-icon-example.md)

[Make every window usable from the keyboard
alone](backlog/keyboard-only-flow-example.md)

[Capture the current architecture](backlog/capture-current-architecture.md)

[Import the existing backlog](backlog/import-existing-backlog.md)

## Low

[Translate the chime names in settings](backlog/localize-chime-names-example.md)

[Draw dark-mode variants of the window
icons](backlog/dark-mode-icons-example.md)

## Deferred

[Save named presets of session and break
lengths](backlog/session-presets-example.md)

[Support the tray on Linux desktops](backlog/linux-tray-support-example.md)

## Done

[Let users turn off the update check](backlog/update-check-opt-out-example.md)

[Rewrite the settings labels in plain
words](backlog/tidy-settings-labels-example.md)

[Stop ticking the timer while no session
runs](backlog/reduce-idle-cpu-example.md)

[Show the remaining time in the tray
tooltip](backlog/statusbar-tooltip-example.md)

## Cancelled

[Shared focus sessions for a team](backlog/team-sessions-example.md)
