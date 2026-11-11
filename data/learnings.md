---
type: hub
description: Durable memory of pitfalls, standing rules, and mistaken beliefs put right, one fact per document.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🧠 Learnings

*What the agent and the team learned the hard way, one fact per document in
`learnings/<slug>.md`, dated when it was discovered: a pitfall and how to avoid
it, a rule the developer laid down, or a belief that proved wrong. A learning
links to the bug, plan or spec it came from instead of repeating it. Group the
links under `##` topic headings once the list outgrows one screen.*

``` chart
title: Learnings per month
height: 160
data: {query: {filter: {type: learning}, project: {created: 1, kind: 1}}}
mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
encoding:
  x: {field: created, timeUnit: utcyearmonth, type: ordinal, title: null, axis: {format: "%b %Y", labelAngle: 0}}
  y: {aggregate: count, type: quantitative, title: Learnings, axis: {tickMinStep: 1}}
  color: {field: kind, type: nominal, title: null, scale: {domain: [trap, rule, correction], range: ["#f76b15", "#0090ff", "#8e4ec6"]}, legend: {orient: top}}
```

## Timer and sound

[Audio must be loaded before the first
chime](learnings/audio-needs-preloading.example)

[Timer tests run on a fake clock](learnings/tests-use-a-fake-clock.example)

[The daily goal counts minutes, not
sessions](learnings/goal-counts-minutes-not-sessions.example)

[A streak day is the local day a session completes
on](learnings/streak-days-use-local-midnight.example)

## Tray, windows and system

[The tray menu reads its labels once, when it
opens](learnings/tray-label-reads-once-on-open.example)

[Shortcut registration fails without an
error](learnings/shortcut-registration-fails-silently.example)

[Do not disturb needs permission before the first
session](learnings/dnd-needs-permission-first.example)

[Settings are not migrated on
update](learnings/settings-are-not-migrated.example)

## Working on the project

[Commit messages name the state
change](learnings/commit-messages-name-the-state-change.example)

[Release notes come from the unreleased
page](learnings/release-notes-come-from-unreleased.example)
