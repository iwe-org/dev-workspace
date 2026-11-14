---
type: tracker
description: Aggregator plans that sequence the work for one release.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🏁 Milestones

*A milestone is a plan whose children are plans: an aggregator document in
`plans/` that inclusion-links the work for one release and sequences it. Each
entry here points at one aggregator. Every child plan also carries the tag
`milestone-<slug>`, which is what the chart counts. The milestone ships when its
last child plan does.*

``` chart
title: Milestone progress
height: {step: 28}
data: {query: {filter: {type: plan, tags: {$exists: true}}, project: {tags: 1, stage: 1}}}
transform:
  - flatten: [tags]
  - filter: "indexof(datum.tags, 'milestone-') == 0"
  - calculate: "slice(datum.tags, 10)"
    as: milestone
  - calculate: "datum.stage == 'done' ? 'Shipped' : datum.stage == 'cancelled' ? 'Cancelled' : datum.stage == 'deferred' ? 'Deferred' : 'Open'"
    as: state
  - calculate: "datum.state == 'Shipped' ? 1 : 0"
    as: shipped
encoding:
  y: {field: milestone, type: nominal, title: null}
layer:
  - mark: {type: bar, cornerRadiusEnd: 3, tooltip: true}
    encoding:
      x: {aggregate: count, type: quantitative, title: Plans, axis: {tickMinStep: 1}}
      color: {field: state, type: nominal, title: null, sort: [Shipped, Open, Deferred, Cancelled], scale: {domain: [Shipped, Open, Deferred, Cancelled], range: ["#30a46c", "#0090ff", "#f76b15", "#e5484d"]}, legend: {orient: top}}
      order: {field: shipped, sort: descending}
  - transform:
      - aggregate: [{op: count, as: total}, {op: sum, field: shipped, as: done}]
        groupby: [milestone]
      - calculate: "datum.done + ' of ' + datum.total"
        as: label
    mark: {type: text, align: left, dx: 6}
    encoding:
      x: {field: total, type: quantitative}
      text: {field: label}
```

[1.0](plans/20260625-v1-milestone-example.md)

[Polish](plans/20260901-polish-milestone-example.md)
