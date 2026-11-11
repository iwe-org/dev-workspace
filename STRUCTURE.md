# Structure

This document explains how the workspace is put together and why — the design
rationale behind the layout. The repo itself is the deliverable: a template
knowledge base a software project copies and fills.

## Layout

```
README.md          # the pitch and quickstart (humans)
index.md           # the start page the iWe app opens first
AGENTS.md          # the operating manual (agents)
SCHEMA.md          # frontmatter reference (humans)
STRUCTURE.md       # this file: design rationale
.iwe/
  config.toml      # markdown conventions + schema→glob bindings
  schemas/         # one <type>.yaml per document type, validated by iwe,
                   # plus a <type>.ui.yaml display sidecar the editor reads
.claude/skills/    # state workflows: setup, explore, plan, implement,
                   # verify, ship, weekly
data/              # the graph
  index.md         # the root hub — every hub is its child
  product.md       # ✏️ fill-in tracker: the doc every session reads first
  <hub>.md + <hub>/  # plans, features, bugs, spec, architecture, concept,
                     # releases, backlog, someday, learnings
                     # (+ milestone.md)
    releases/metrics.csv  # one row of numbers per release, drawn by a chart
  plans-archive.md   # closed plans: done by month, and cancelled
  bugs-archive.md    # closed bugs: fixed, and cancelled
```

## Design decisions

**Hub + directory; membership is a link.** Each entity type pairs a hub file
(`data/plans.md`) with a directory (`data/plans/`). What makes a document a plan
is the inclusion link from the hub, not its path — the directory is storage, the
graph is truth. This keeps categorization refactorable (move a link, not a file)
and makes every hub a queryable index.

**Work items vs. reference.** Plans, features, bugs, releases, and backlog tasks
are *work items*: a lifecycle `stage`, dates, a commit gate
(`iwe schema validate`). Specs, architecture, concept, someday, and learnings
docs are *durable reference*: they carry a `type` and the shared OKF families,
but no `stage`, because nothing about them is a lifecycle. The split keeps the
ceremony where it pays. Every document is validated — OKF conformance requires a
`type` everywhere — but only work items are asked for more than that.

**Dual representation.** A work item's stage lives in frontmatter (for queries)
and as its link's position in the hub (for reading). The rule that makes this
survivable: change both together, and no item is ever delisted — `## Done` and
`## Cancelled` are history, not a trash can. Frontmatter wins any disagreement:
a plan whose body says it shipped but whose `stage` is empty is still active as
far as every hub and query can tell, so closing a plan is part of the change
that finishes it.

**Archive hubs for closed work.** Plans and bugs accumulate fastest, and an open
list buried under hundreds of closed items stops being read. So each has a
second hub, `plans-archive.md` and `bugs-archive.md`, and closing an item moves
its link there. Only the link moves — the document keeps its key and every
reference to it — so the archive costs nothing the graph doesn't already do, and
a check for a closed item still on an open list is a one-line query.

**Minimal frontmatter; relationships are links.** Five stage vocabularies, three
date fields, and a few fields that each answer one question a query needs — who
reported a bug and on which versions, which tests guard a feature, what kind of
fact a learning is — that's the entire frontmatter surface. Everything
relational (plan → spec, bug → requirement, release → feature, plan → plan
dependency) is a markdown link in the body, which keeps relationships visible
while reading and traversable via `iwe find --references`.

**The funnel.** Ideas harden stepwise: `someday/` (no commitment) → `backlog/`
(prioritized) → `plans/YYYYMMDD-<slug>` (committed, dated) → feature
`implemented` → `releases/unreleased` → a cut release. Each promotion is just a
new doc plus a moved link.

**Spec sync at ship.** A plan names the specs it will touch (`## Spec changes`);
the ship skill updates those specs *before* the plan's stage flips. This is the
single strongest defense against the failure mode of every long-lived doc set:
specs that describe last quarter's behavior.

**Milestones are plans of plans.** An aggregator plan inclusion-links its child
plans and sequences them; `milestone.md` links the aggregators. No special
milestone machinery — the graph already does it.

**Code anchors are dated.** `path:line — symbol` lists under
`## Key references`, stamped `Verified anchor points (line numbers as of ...)`.
Line numbers rot; the stamp tells the reader how much to trust them, and the
rule ("from the current checkout, never from memory") keeps them honest at write
time.

**Tickets come first, fixes second.** A ticket opened before any code changes,
describing what the person saw rather than what the agent suspects, is where the
reasoning behind a fix outlives the session that made it. `reported_by`
separates what people reported from what agents found on their own;
`affected_versions` turns "this came back" into a query, because a returning bug
reopens its ticket instead of starting a second one. The body's sections are
schema-enforced, so every ticket answers the same three questions — what
happened, what should have, how to see it — before anything else.

**A feature claims its guard tests.** `stage: implemented` alone is a claim with
no evidence. Requiring `tests` on an implemented feature makes the gap between
"shipped" and "protected" visible at validation time instead of at the next
regression.

**Learnings are memory, one fact at a time.** Pitfalls, standing rules and
mistaken beliefs put right are what a new session most needs and is least likely
to find in code. Each gets one short dated page, linked from `learnings.md` by
topic, so it can be found, cited and corrected individually rather than lost in
a long notes file.

**Charts read the frontmatter.** Each hub opens with live charts whose data is a
query over the documents — never pasted numbers — so a chart is exactly as true
as the frontmatter it reads. They select by `type` and fields rather than by hub
membership, which keeps a moved link from changing a count, and they are the
main payoff of keeping dates and stages honest: a closed plan without
`completed` simply never appears on the shipped-per-month bar. Numbers that live
outside the documents, such as test counts per release, go in a small CSV beside
the docs and join to the documents through a lookup.

**Display lives beside validation.** Each schema has a `<type>.ui.yaml` sidecar
with titles, colors and hub columns. The validator never reads it, so display
choices can't weaken the gate, and the schema stays the one source of fields and
values.

**Authoring rules travel with the product doc.** `data/product.md` carries an
optional `## Authoring rules` section — per-document-type constraints the plan
and ship skills consult before writing. Project-specific discipline lives in
data, not in forked skill prompts.

## Influences: OpenSpec

Adopted, in IWE-native form:

- **Spec sync at ship** — OpenSpec's archive step folds a change's spec deltas
  into the canonical specs; here the ship skill does the same walk over
  `## Spec changes`.
- **Requirement/Scenario format** — `### Requirement:` + SHALL +
  `#### Scenario:` WHEN/THEN, with OpenSpec's "progressive rigor" stance: scale
  ceremony with risk, as a convention rather than a validation gate.
- **Authoring rules** — modeled on OpenSpec's per-artifact `rules:` config,
  relocated into the product doc.
- **Dependencies** — OpenSpec's `dependsOn` metadata becomes a `## Depends on`
  section of inline links: queryable, no new machinery.
- **The explore / implement / verify skills** — adapted from OpenSpec's explore
  (thinking-partner stance, never writes code), apply/continue (task-by-task
  execution with artifact discipline), and verify-change (completeness /
  correctness / coherence report) workflows, translated onto the
  single-plan-document model.

Deliberately not adopted:

- **Archive by file move** (`changes/archive/YYYY-MM-DD-*`) — `stage: done` plus
  a link moved to an archive hub gives the same lifecycle without moving files
  or breaking links.
- **Per-change file bundles** (proposal/design/tasks/specs per change) — one
  plan document with sections is leaner; when a plan genuinely outgrows one
  file, inclusion links split it natively.
- **Delta headers** (`## ADDED/MODIFIED/REMOVED Requirements`) — machinery for a
  parser this workspace doesn't need; `iwe` handles renames and references.

## Publishing

Intended to live as a GitHub template repository. The runtime artifacts
(databases, snapshots) live in `.iwe/app/` and are gitignored; only
`config.toml` and the schemas are authored files.
