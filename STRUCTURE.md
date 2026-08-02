# Structure

This document explains how the workspace is put together and why — the design
rationale behind the layout. The repo itself is the deliverable: a template
knowledge base a software project copies and fills.

## Provenance

The structure is generalized from a production app-development knowledge base
(hundreds of documents, maintained daily by agents) built on IWE; the template
conventions — root docs, fill-in tracker, `*.example.md` demo set, workflow
skills — follow its sibling, the IWE marketing workspace template. Two ideas are
adapted from [OpenSpec](https://github.com/Fission-AI/OpenSpec) (see Influences
below).

## Layout

```
README.md          # the pitch and quickstart (humans)
AGENTS.md          # the operating manual (agents) — CLAUDE.md points here
SCHEMA.md          # frontmatter reference (humans)
STRUCTURE.md       # this file: design rationale
.iwe/
  config.toml      # markdown conventions + schema→glob bindings
  schemas/         # validation schemas: plan, feature, bug, release, task, codebase
.claude/skills/    # state workflows: setup, explore, plan, implement,
                   # verify, ship, weekly
data/              # the graph
  index.md         # the root hub — every hub is its child
  product.md       # ✏️ fill-in tracker: the doc every session reads first
  <hub>.md + <hub>/  # plans, features, bugs, spec, architecture, concept,
                     # codebase, releases, backlog, someday (+ milestone.md)
```

## Design decisions

**Hub + directory; membership is a link.** Each entity type pairs a hub file
(`data/plans.md`) with a directory (`data/plans/`). What makes a document a plan
is the inclusion link from the hub, not its path — the directory is storage, the
graph is truth. This keeps categorization refactorable (move a link, not a file)
and makes every hub a queryable index.

**Work items vs. reference.** Plans, features, bugs, releases, and backlog tasks
are *work items*: a lifecycle `stage`, dates, a commit gate
(`iwe schema validate`). Specs, architecture, concept, and someday docs are
*durable reference*: they carry a `type` and the shared OKF families, but no
`stage`, because nothing about them is a lifecycle. The split keeps the ceremony
where it pays. Every document is validated — OKF conformance requires a `type`
everywhere — but only work items are asked for more than that.

**Dual representation.** A work item's stage lives in frontmatter (for queries)
and as its link's position in the hub (for reading). The rule that makes this
survivable: change both together, and no item is ever delisted — `## Done` and
`## Cancelled` are history, not a trash can.

**Minimal frontmatter; relationships are links.** Five stage vocabularies and
three date fields — that's the entire frontmatter surface. Everything relational
(plan → spec, bug → requirement, release → feature, plan → plan dependency) is a
markdown link in the body, which keeps relationships visible while reading and
traversable via `iwe find --references`.

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

**The codebase map is derived truth.** `data/codebase/` is a fourth kind of
truth next to spec (must), architecture (why), and concept (why at all): *what
the code is*, written only from reading it. The map mirrors the code's
containment tree: every component gets a doc at a canonical key matching its
source path, and parents inclusion-link children (`## Contains`), so
`iwe tree -k data/codebase` renders the code's structure. Map docs carry the
most provenance of any type — `source` (the code described), `commit` (the
revision it was read at), and OKF's `verified` (who last confirmed the doc
against that code) — because provenance is what makes incremental refresh cheap:
`git log <commit>..HEAD -- <source>` finds exactly the dirty docs. Each
relationship is written in one direction only (`## Contains` down,
`## Depends on` out); "part of" and "used by" are backlink queries, so they can
never rot. Map docs are freely rewritable from a fresh read of the code;
decision records never are.

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
  graph queries gives the same lifecycle without moving files or breaking links.
- **Per-change file bundles** (proposal/design/tasks/specs per change) — one
  plan document with sections is leaner; when a plan genuinely outgrows one
  file, inclusion links split it natively.
- **Delta headers** (`## ADDED/MODIFIED/REMOVED Requirements`) — machinery for a
  parser this workspace doesn't need; `iwe` handles renames and references.

## Improvements over the source KB

Lessons from the knowledge base this template generalizes:

- Its `## Failed` plans section had no frontmatter counterpart — failed plans
  were indistinguishable from active ones in queries. Here `stage: cancelled` ⇄
  `## Cancelled` covers abandoned work completely.
- It accumulated overlapping ad-hoc hubs (dated snapshot indexes duplicating the
  real ones). This template ships exactly one hub per entity type; point-in-time
  views should be queries, not files.
- Its examples of convention lived in real (product-specific) docs. Here the
  `*.example.md` set demonstrates every convention on a fictional product,
  schema-validated so it can't rot, and deleted by setup.

## Publishing

Intended to live as a GitHub template repository. The `.iwe/` runtime artifacts
(databases, snapshots) are gitignored; only `config.toml` and the schemas are
authored files.
