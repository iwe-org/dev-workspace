# Frontmatter reference

The human-readable companion to the machine schemas. Validation lives in
`.iwe/schemas/*.yaml`, bound to key globs in `.iwe/config.toml` (`[schemas]`
section) — bindings are by path, and **every** document under `data/` is bound,
hubs included. Run `iwe schema validate` before committing — exit 0 is the gate.

The examples below describe one fictional product (Pomodux, a focus-timer app);
the `*-example.md` docs (`releases/example-<version>.md` for releases)
demonstrate each shape in place.

`data/` is a conformant [Open Knowledge
Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle. That is why every document carries a `type` and why the reserved
`data/index.md` and `data/log.md` have shapes of their own. The per-type blocks
below show only the fields specific to that type; the shared families come next.

## Fields every type carries

OKF's optional families (SPEC §4.1, §5), declared on every type's schema:

- `description` — one sentence. Every document has one; it is what an OKF
  consumer and `data/index.md` show before opening the file.
- `generated` — `{ by, at }`. Write it on **every** edit. `by` is an actor (SPEC
  §7): `claude-code/opus-5` for agent writes, `human:<handle>` for hand-authored
  or human-confirmed content, `process:<id>` for automation.
- `sources` — the materials a document derives from, each
  `{ id, resource, title, author, usage_count, last_modified }` with `resource`
  required. A `resource` may be a URL or a repo path. Attribute individual
  claims with a markdown footnote keyed to an entry's `id` (SPEC §5.1).
- `verified` — `{ by, at }` or a list, recording who confirmed the content
  against its sources.
- `resource`, `tags`, `stale_after`, `usage_window` — optional. Set
  `stale_after` on anything that goes stale when the code moves.

### `stage` versus `status`

`stage` is this workspace's workflow field — the per-type enums below. `status`
is OKF's lifecycle field and takes only `draft | stable | deprecated`; absent
means stable, which is why most documents omit it. Maintain the two together:
whenever you set `stage`, derive `status` from this table and set or clear it.

| type    | `stage` value             | set `status` |
| ------- | ------------------------- | ------------ |
| bug     | `cancelled`               | `deprecated` |
| feature | `proposed`                | `draft`      |
| feature | `deprecated`, `cancelled` | `deprecated` |
| plan    | `cancelled`               | `deprecated` |
| release | `unreleased`              | `draft`      |
| task    | `cancelled`               | `deprecated` |

Every other `stage` value means the document is current — omit `status`. The
reference types and learnings never set it.

## Reserved files

`data/index.md` and `data/log.md` are OKF reserved filenames and are not concept
documents — they carry no `type` and are checked by `okf-index.yaml` and
`okf-log.yaml` instead.

- `data/index.md` — the bundle-root index. Its only frontmatter is
  `okf_version: "0.2"`. The body is `#` sections of link bullets, each bullet a
  markdown link to a document followed by `-` and that document's `description`.
- `data/log.md` — the update history. A title section over `## YYYY-MM-DD`
  groups of bullets, newest first. The `ship` skill appends to it.

## Plans — `data/plans/YYYYMMDD-<slug>.md`

Implementation plans, migrations, and design proposals.

``` yaml
---
type: plan          # required
stage: done         # approved | deferred | done | cancelled — omit while active/pending
created: 2026-07-01 # required, YYYY-MM-DD
completed: 2026-07-18 # required when stage: done
---
```

Body: `## Context`, `## Approach`, `## Implementation Steps` (`### Task N`,
`**Files:**`, checkboxes), `## Spec changes`, `## Depends on` (optional),
`## Verification`, `## Out of scope`, `## Key references` (dated
`path:line — symbol` anchors).

Whether a plan shipped is answered by `stage` and nothing else. Hubs and queries
never look at the body, so a fully ticked checklist or a "finished" remark under
a plan with no stage still reads as active work. An open plan (no stage,
`approved`) is listed under `## Active` in `data/plans.md`, a `deferred` one
under `## Deferred`. A `done` or `cancelled` plan moves to
`data/plans-archive.md` in the same change — `## Done` under the month it
shipped, or `## Cancelled` — and a milestone that lists it moves its link to the
milestone's `## Shipped`. A cancelled plan's first line points to its successor
or gives the reason it was dropped. When one plan takes over from another, both
edits land together: the newcomer links back, and the old plan is cancelled.

A plan that a milestone schedules also carries `tags: [milestone-<slug>]`
(`milestone-mvp`, `milestone-polish`). The inclusion link in the milestone plan
is what you read; the tag is what the milestone progress chart counts.

## Features — `data/features/<slug>.md`

Feature design documents; the stage carries the whole lifecycle.

``` yaml
---
type: feature      # required
stage: implemented # proposed | accepted | implemented | deprecated | cancelled — required
tests:             # required when stage: implemented
  - src/timer/engine.test.ts
---
```

`tests` names the **guard tests**: the test files, suites or test names that
fail when the behaviour the feature describes breaks, written the way the
project's test runner takes them. Marking a feature implemented without naming
any test fails `iwe schema validate`. Treat the failure as missing protection:
write the test, then list it.

Body: `## Purpose`, `## Behaviour`, `## Edge cases`, `## Open questions`.

## Bugs — `data/bugs/<slug>.md`

One ticket per problem, opened before any fix is written, in the words of
whoever hit it.

``` yaml
---
type: bug             # required
stage: done           # deferred | done | cancelled — omit while open
created: 2026-07-27   # required, when the ticket was opened
completed: 2026-07-29 # required when stage: done
reported_by: agent    # required: user | agent
affected_versions:    # required: versions it showed up in, release order
  - 0.1.0             # a released version, or unreleased
---
```

- `reported_by` records who noticed the problem, not who typed the ticket:
  `user` for a person, `agent` for an agent that spotted it unprompted — in a
  review, a red test run, or during unrelated work. The Symptom's first sentence
  gives the date, the version, and who noticed it.
- `affected_versions` is what makes a recurring bug visible. A bug that returns
  is **reopened**: append the new version, drop `stage`, add a dated
  `## Recurrence` entry explaining why the first fix did not hold, and move the
  link back to `data/bugs.md`. Listing the repeat offenders takes one query:
  `iwe find --filter '{type: bug, affected_versions: {$size: {$gte: 2}}}' -f keys`.

Body, enforced by the schema in this order: one `# Bug: <one-line symptom>`
heading holding `## Symptom`, `## Expected`, `## Reproduction` (numbered steps,
or a sentence saying none are known yet), then optionally `## Leads` (suspected
causes, each with the quickest check that would confirm or rule it out),
`## Root cause`, `## Fix`, `## Recurrence`, `## Related`, `## Key references`.
No other section is accepted. The `## Fix` section is three bullets: **Change**
(what was touched), **Guard** (the test that fails without the fix, or why none
exists), **Shipped** (the commit and the release entry), with the root cause in
its own section.

Open tickets are listed under `## Open` in `data/bugs.md` and deferred ones
under `## Deferred`; a ticket closed as `done` moves to `## Fixed` in
`data/bugs-archive.md`, one closed as `cancelled` (by design, a duplicate, or
impossible to trigger again — the `## Fix` section explains which) to
`## Cancelled`. A closed ticket stays in the graph.

## Releases — `data/releases/<version>.md`

One page per version; `unreleased.md` accumulates until a release is cut.

``` yaml
---
type: release     # required
version: 0.1.0    # required: a semantic version; the accumulator page uses "unreleased"
date: 2026-07-18  # required when stage: released
stage: released   # released | unreleased — required
---
```

Body: `## Added` / `## Fixed` (/ `## Changed`) as inclusion links to feature and
bug docs.

## Backlog tasks — `data/backlog/<slug>.md`

Atomic prioritized work items waiting to become plans.

``` yaml
---
type: task          # required
stage: planned      # planned | deferred | done | cancelled — required
priority: high      # high | medium | low
created: 2026-08-01 # required, YYYY-MM-DD
completed: 2026-08-14 # required when stage: done
---
```

The link in `data/backlog.md` sits under the priority section while the task is
`planned`, and under `## Deferred`, `## Done` or `## Cancelled` otherwise.

## Learnings — `data/learnings/<slug>.md`

Durable memory, one fact per page: a **trap** (a pitfall and how to avoid it), a
**rule** (a standing instruction from the developer), or a **correction** (a
belief that proved wrong, and what holds instead). The body is exactly one
heading — the fact as a sentence — with the fact as prose under it, at most
about 600 tokens. A learning links to the bug, plan or spec it came from instead
of repeating it.

``` yaml
---
type: learning      # required
created: 2026-07-12 # required, when the fact was discovered or stated
kind: trap          # trap | rule | correction
session: <id>       # optional, the working session it came from
---
```

Link each learning from `data/learnings.md` under a `##` topic heading. A
learning that turns out wrong is rewritten or deleted (`iwe delete`), not left
beside its correction.

## Reference documents — type only

`data/spec/`, `data/architecture/`, `data/concept/`, and `data/someday/` docs
carry a `type` (`spec`, `architecture`, `concept`, `someday`) and nothing else
that is required: they are durable reference, not work items, so no `stage`
applies. Specs use the Requirement/Scenario format (`### Requirement:` + SHALL,
`#### Scenario:` + WHEN/THEN); architecture notes record decisions with their
rejected alternatives.

## Hubs and the trackers

Hub files (`data/plans.md`, `data/features.md`, …) carry `type: hub`;
`data/product.md` and `data/milestone.md` carry `type: tracker`. Both take
`stage: living`. A hub's body is inclusion links grouped by `##` sections; a
document's category is which hub links it, not which directory holds it.

Plans and bugs have an **archive hub** each, `data/plans-archive.md` and
`data/bugs-archive.md`, also `type: hub`. The main hub lists the open items and
the archive the closed ones, so the open list stays short enough to read while
every closed item stays reachable from the graph. Closing an item moves its
link; the document itself never moves.

## Charts — ```` ``` chart ```` blocks

The hubs draw live charts from the frontmatter above. A chart block is one
Vega-Lite spec written in YAML, and every `data` in it is a query or a file:

- `{query: {filter, project, sort, limit}}` — the same filter language as
  `iwe find --filter` (`$exists`, `$in`, `$size`, `$gte`, …). Each row is the
  document `key` plus the projected fields; `$title` and `$content` project the
  title and the body.
- `{url: releases/metrics.csv}` — a CSV, TSV, JSON or JSONL file beside the
  docs, resolved from the document's folder. A `lookup` transform joins its rows
  to a query's.
- `{name: <dataset>}` — an entry of the spec's top-level `datasets`, for data
  two panels share.

Inline `values` are refused, so a chart can only show what the documents say.
The template's charts select by `type` and frontmatter fields rather than by hub
links, so moving a link never changes a number. What each hub draws:

| Hub                     | Charts                                                                                                                                                   |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data/bugs.md`          | filed and fixed per month; open bugs, recurring bugs and open bugs by `reported_by`; bugs whose `affected_versions` hold two or more versions            |
| `data/bugs-archive.md`  | closed, fixed and cancelled counts; fixes per month by `reported_by` with the mean days to fix                                                           |
| `data/plans.md`         | every plan by `stage`                                                                                                                                    |
| `data/plans-archive.md` | shipped and cancelled counts; plans shipped per month                                                                                                    |
| `data/backlog.md`       | tasks by `stage` and `priority`; tasks added per month                                                                                                   |
| `data/features.md`      | features by `stage`; guard tests listed                                                                                                                  |
| `data/releases.md`      | items per release on a timeline, counted from each page's `## Added`, `## Changed` and `## Fixed`; tests and suite time from `data/releases/metrics.csv` |
| `data/learnings.md`     | learnings per month by `kind`                                                                                                                            |
| `data/milestone.md`     | each milestone's plans by stage, from the `milestone-<slug>` tags                                                                                        |

`data/releases/metrics.csv` has one row per released version (`version`,
`tests`, `suite_seconds`, `app_mb`); its `version` column joins to the release
pages' `version` field.

## Display sidecars — `.iwe/schemas/<name>.ui.yaml`

Each schema has a sidecar of the same name that the iWe editor reads to draw the
frontmatter band and the inclusion rows of a hub. The validator never reads it,
so it can only decorate what the schema already declares:

- `name` — the entry's display name (`Plans`, `Bugs`, …).
- `columns` — the fields a hub row shows for a child of this type, in order
  (`[stage, completed]` for plans, `[stage, reported_by]` for bugs).
- `fields.<key>` — `title`, `description`, `hidden`, `multiline`, and `values`:
  for each enum value a `title`, `description` and `color`.

The provenance families (`generated`, `verified`, `sources`, `resource`,
`usage_window`) and the constant `type` are hidden — the agent maintains them.
When you add a field or an enum value to a schema, add its decoration to the
sidecar in the same change.
