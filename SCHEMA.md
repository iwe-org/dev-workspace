# Frontmatter reference

The human-readable companion to the machine schemas. Validation lives in
`.iwe/schemas/*.yaml`, bound to key globs in `.iwe/config.toml` (`[schemas]`
section) — bindings are by path, and **every** document under `data/` is bound,
hubs included. Run `iwe schema validate` before committing — exit 0 is the gate.

The examples below describe one fictional product (Pomodux, a focus-timer app);
the `*.example.md` docs demonstrate each shape in place.

`data/` is a conformant [Open Knowledge
Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle. That is why every document carries a `type` and why the reserved
`data/index.md` and `data/log.md` have shapes of their own. The per-type blocks
below show only the fields specific to that type; the shared families come next.

## Fields every type carries

OKF's optional families (SPEC §4.1, §5), declared on every schema and checked by
`.iwe/schemas/okf.yaml`:

- `description` — one sentence. Every document has one; it is what an OKF
  consumer and `data/index.md` show before opening the file.
- `generated` — `{ by, at }`. Write it on **every** edit. `by` is an actor (SPEC
  §7): `claude-code/opus-5` for agent writes, `human:<handle>` for hand-authored
  or human-confirmed content, `process:<id>` for automation.
- `sources` — the materials a document derives from, each
  `{ id, resource, title, author, usage_count, last_modified }` with `resource`
  required. A `resource` may be a URL or a repo path, so a codebase map cites
  the code it describes. Attribute individual claims with a markdown footnote
  keyed to an entry's `id` (SPEC §5.1).
- `verified` — `{ by, at }` or a list, recording who confirmed the content
  against its sources. Codebase maps use it for "this doc still matches the
  code", which is what the old date-valued `verified` field meant.
- `resource`, `tags`, `stale_after`, `usage_window` — optional. Set
  `stale_after` on codebase maps, which go stale the moment the code moves.

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

Every other `stage` value means the document is current — omit `status`. `task`
and the reference types never set it.

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
stage: done         # done | cancelled — omit while active/pending
created: 2026-07-01 # required, YYYY-MM-DD
completed: 2026-07-18 # required when stage: done
---
```

Body: `## Context`, `## Approach`, `## Implementation Steps` (`### Task N`,
`**Files:**`, checkboxes), `## Spec changes`, `## Depends on` (optional),
`## Verification`, `## Out of scope`, `## Key references` (dated
`path:line — symbol` anchors). The link in `data/plans.md` sits under the
section matching the stage.

## Features — `data/features/<slug>.md`

Feature design documents; the stage carries the whole lifecycle.

``` yaml
---
type: feature      # required
stage: implemented # proposed | accepted | implemented | deprecated | cancelled — required
---
```

Body: `## Purpose`, `## Behaviour`, `## Edge cases`, `## Open questions`.

## Bugs — `data/bugs/<slug>.md`

Bug reports and investigations. H1 starts with `Bug:`.

``` yaml
---
type: bug   # required
stage: done # done | cancelled — omit while open
---
```

Body: `## Symptom`, `## Reproduction`, `## Root cause`, `## Fix`,
`## Key references`.

## Releases — `data/releases/<version>.md`

One page per version; `unreleased.md` accumulates until a release is cut.

``` yaml
---
type: release     # required
version: 0.1.0    # required; the accumulator page uses "unreleased"
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
stage: planned      # planned | done — required
priority: high      # high | medium | low
created: 2026-08-01 # required, YYYY-MM-DD
completed: 2026-08-14 # set when done
---
```

## Codebase map — `data/codebase/<canonical-path>.md`

Derived docs, written only by reading the code: one per component (crate,
package, module), plus `flow-<name>` and `api-<name>`. Keys are canonical — a
component's key mirrors its source path with wrapper segments elided
(`crates/liwe/src/graph` → `data/codebase/crates/liwe/graph`). Their extra
fields are provenance, not lifecycle.

``` yaml
---
type: codebase        # required
source: src/timer     # required; one path, or a list of paths (first is primary)
commit: "3f1a9c2"     # required, always quoted — an all-digit SHA parses as a number
verified: { by: human:author, at: 2026-07-25T00:00:00Z }  # required; who last confirmed the doc against the code
stale_after: 2026-11-01 # the day to re-check it
---
```

Component body: role paragraph, `## Contains` (inclusion links to children —
this builds the tree), `## Public surface`, `## How it works`, `## Depends on`
(one-way — "used by" is a backlink query), `## Invariants & gotchas`,
`## Key references`. Flows: numbered `## Trace` + `## Failure modes`.

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
