---
type: hub
description: One page per version, aggregating the features and fixes it shipped.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🚀 Releases

*One page per shipped version in `releases/<version>.md`, newest first, each
aggregating its features and fixes as inclusion links. `releases/unreleased.md`
accumulates work as it ships; cutting a release renames it to the version number
and starts a fresh accumulator (the ship skill does this). The timeline counts
the links under each page's `## Added`, `## Changed` and `## Fixed`; the second
chart joins `releases/metrics.csv`, one row per version, to the release dates.*

``` chart
title: Items per release
height: 200
data: {query: {filter: {type: release, stage: released}, project: {version: 1, date: 1, body: $content}}}
transform:
  - calculate: "indexof(datum.body, '\\n## Added')"
    as: a
  - calculate: "datum.a < 0 ? '' : slice(datum.body, datum.a + 9)"
    as: aRest
  - calculate: "indexof(datum.aRest, '\\n## ') < 0 ? datum.aRest : slice(datum.aRest, 0, indexof(datum.aRest, '\\n## '))"
    as: aSection
  - calculate: "indexof(datum.body, '\\n## Changed')"
    as: c
  - calculate: "datum.c < 0 ? '' : slice(datum.body, datum.c + 11)"
    as: cRest
  - calculate: "indexof(datum.cRest, '\\n## ') < 0 ? datum.cRest : slice(datum.cRest, 0, indexof(datum.cRest, '\\n## '))"
    as: cSection
  - calculate: "indexof(datum.body, '\\n## Fixed')"
    as: f
  - calculate: "datum.f < 0 ? '' : slice(datum.body, datum.f + 9)"
    as: fRest
  - calculate: "indexof(datum.fRest, '\\n## ') < 0 ? datum.fRest : slice(datum.fRest, 0, indexof(datum.fRest, '\\n## '))"
    as: fSection
  - calculate: "length(split(datum.aSection, '\\n[')) - 1"
    as: Added
  - calculate: "length(split(datum.cSection, '\\n[')) - 1"
    as: Changed
  - calculate: "length(split(datum.fSection, '\\n[')) - 1"
    as: Fixed
  - fold: [Added, Changed, Fixed]
    as: [kind, items]
encoding:
  x: {field: date, type: temporal, timeUnit: utcyearmonthdate, title: null, axis: {format: "%b %-d", labelAngle: 0}}
layer:
  - mark: {type: bar, width: 14, tooltip: true}
    encoding:
      y: {field: items, type: quantitative, stack: zero, title: Items, axis: {tickMinStep: 1}}
      color: {field: kind, type: nominal, title: null, scale: {domain: [Added, Changed, Fixed], range: ["#0090ff", "#f76b15", "#30a46c"]}, legend: {orient: top}}
      tooltip:
        - {field: version, title: Version}
        - {field: kind, title: Kind}
        - {field: items, title: Items}
  - transform:
      - aggregate: [{op: sum, field: items, as: total}]
        groupby: [date, version]
    mark: {type: text, dy: -8, fontSize: 11}
    encoding:
      y: {field: total, type: quantitative}
      text: {field: version}
```

``` chart
title: Tests and suite time per release
height: 180
datasets:
  metrics: {url: releases/metrics.csv}
  releases: {query: {filter: {type: release, stage: released}, project: {version: 1, date: 1}}}
data: {name: metrics}
transform:
  - lookup: version
    from: {data: {name: releases}, key: version, fields: [date]}
  - filter: "datum.date != null"
  - calculate: "toDate(datum.date)"
    as: shipped
encoding:
  x: {field: shipped, type: temporal, timeUnit: utcyearmonthdate, title: null, axis: {format: "%b %-d", labelAngle: 0}}
layer:
  - mark: {type: bar, width: 14, color: "#0090ff", opacity: 0.5, tooltip: true}
    encoding:
      y: {field: tests, type: quantitative, title: Tests}
      tooltip:
        - {field: version, title: Version}
        - {field: tests, title: Tests}
        - {field: suite_seconds, title: Suite seconds}
  - mark: {type: line, point: true, color: "#f76b15"}
    encoding:
      y: {field: suite_seconds, type: quantitative, title: Suite seconds}
resolve: {scale: {y: independent}}
```

[Unreleased](releases/unreleased.md)

[0.4.0](releases/0.4.0.example)

[0.3.1](releases/0.3.1.example)

[0.3.0](releases/0.3.0.example)

[0.2.0](releases/0.2.0.example)

[0.1.0](releases/0.1.0.example)
