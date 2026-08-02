# Frontmatter reference

The human-readable companion to the machine schemas. Validation lives in
`.iwe/schemas/*.yaml`, bound to key globs in `.iwe/config.toml` (`[schemas]`
section) — bindings are by path, and the extension-less hub keys (`data/plans`,
`data/bugs`, …) match no glob, so hubs stay unvalidated by design. Run
`iwe schema validate` before committing — exit 0 is the gate.

The examples below describe one fictional product (Pomodux, a focus-timer app);
the `*.example.md` docs demonstrate each shape in place.

## Plans — `data/plans/YYYYMMDD-<slug>.md`

Implementation plans, migrations, and design proposals.

``` yaml
---
status: done        # done | cancelled — omit while active/pending
created: 2026-07-01 # required, YYYY-MM-DD
completed: 2026-07-18 # required when status: done
---
```

Body: `## Context`, `## Approach`, `## Implementation Steps` (`### Task N`,
`**Files:**`, checkboxes), `## Spec changes`, `## Depends on` (optional),
`## Verification`, `## Out of scope`, `## Key references` (dated
`path:line — symbol` anchors). The link in `data/plans.md` sits under the
section matching the status.

## Features — `data/features/<slug>.md`

Feature design documents; the status carries the whole lifecycle.

``` yaml
---
status: implemented # proposed | accepted | implemented | deprecated | cancelled — required
---
```

Body: `## Purpose`, `## Behaviour`, `## Edge cases`, `## Open questions`.

## Bugs — `data/bugs/<slug>.md`

Bug reports and investigations. H1 starts with `Bug:`.

``` yaml
---
status: done # done | cancelled — omit while open
---
```

Body: `## Symptom`, `## Reproduction`, `## Root cause`, `## Fix`,
`## Key references`.

## Releases — `data/releases/<version>.md`

One page per version; `unreleased.md` accumulates until a release is cut.

``` yaml
---
version: 0.1.0    # required; the accumulator page uses "unreleased"
date: 2026-07-18  # required when status: released
status: released  # released | unreleased — required
---
```

Body: `## Added` / `## Fixed` (/ `## Changed`) as inclusion links to feature and
bug docs.

## Backlog tasks — `data/backlog/<slug>.md`

Atomic prioritized work items waiting to become plans.

``` yaml
---
status: planned   # planned | done — required
priority: high    # high | medium | low
created: 2026-08-01 # required, YYYY-MM-DD
completed: 2026-08-14 # set when done
---
```

## Codebase map — `data/codebase/<canonical-path>.md`

Derived docs written by the map skill: one per component (crate, package,
module), plus `flow-<name>` and `api-<name>`. Keys are canonical — a component's
key mirrors its source path with wrapper segments elided
(`crates/liwe/src/graph` → `data/codebase/crates/liwe/graph`). The only
reference-type docs with frontmatter — provenance, not lifecycle.

``` yaml
---
source: src/timer     # required; one path, or a list of paths (first is primary)
commit: "3f1a9c2"     # required, always quoted — an all-digit SHA parses as a number
verified: 2026-07-25  # required, YYYY-MM-DD; when the doc last matched the code
---
```

Component body: role paragraph, `## Contains` (inclusion links to children —
this builds the tree), `## Public surface`, `## How it works`, `## Depends on`
(one-way — "used by" is a backlink query), `## Invariants & gotchas`,
`## Key references`. Flows: numbered `## Trace` + `## Failure modes`.

## Reference documents — no frontmatter

`data/spec/`, `data/architecture/`, `data/concept/`, and `data/someday/` docs
carry **no frontmatter**: they are durable reference, not work items — nothing
about them is a lifecycle. Specs use the Requirement/Scenario format
(`### Requirement:` + SHALL, `#### Scenario:` + WHEN/THEN); architecture notes
record decisions with their rejected alternatives.

## Hubs and the tracker

Hub files (`data/plans.md`, `data/features.md`, …) and `data/product.md` carry
no frontmatter. A hub's body is inclusion links grouped by `##` sections; a
document's category is which hub links it, not which directory holds it.
