# Changelog

All notable changes to this template are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

This is a template you clone, so the entries below double as migration notes: if
you cloned an earlier copy, they tell you what to change in your own workspace.

## [1.2.0]

### Added

- **Learnings** — a `learning` type for durable memory, one fact per page: a
  pitfall to avoid, a standing rule, or a mistaken belief put right, with its
  `created` date and a `kind`. `data/learnings.md` lists them by topic, and two
  example learnings show the shape. The agent consults learnings before working
  on an area and writes one when something was learned the hard way.
- **Archive hubs for closed work** — `data/plans-archive.md` (done plans by
  month, cancelled plans) and `data/bugs-archive.md` (fixed and cancelled
  tickets). Closing a plan or bug moves its link there, so the open lists stay
  short and nothing is delisted.
- **Display sidecars** — a `<type>.ui.yaml` beside every schema, read by the iWe
  editor: display names, hub columns (`stage` and `completed` for plans, `stage`
  and `reported_by` for bugs, …), value titles and colors, and the provenance
  fields hidden from the frontmatter band. The validator never reads them.
- Bug tickets carry `created`, `reported_by` (`user` or `agent`) and
  `affected_versions`, all required, and `completed` once fixed. A bug that
  returns is reopened rather than filed again, so repeat offenders can be listed
  with one query.
- Bug bodies are schema-checked: one `# Bug:` heading holding `## Symptom`,
  `## Expected` and `## Reproduction`, then optional `## Leads`,
  `## Root cause`, `## Fix`, `## Recurrence`, `## Related` and
  `## Key references`, in that order.
- Features marked `implemented` name their guard tests in `tests`.
- **Live charts on the hubs** — ```` ``` chart ```` blocks (Vega-Lite specs over
  queries) on bugs, the bugs archive, plans, the plans archive, backlog,
  features, releases, learnings and milestones: bugs filed and fixed per month,
  open and recurring bugs, plans shipped per month, stage mixes, a release
  timeline, learnings per kind and milestone progress. They select documents by
  `type` and frontmatter, so they need no upkeep.
- `data/releases/metrics.csv` — test count and suite time per release, joined to
  the release pages in a chart. Setup cuts it down to its header line.
- Plans a milestone schedules carry a `milestone-<slug>` tag, which the
  milestone progress chart counts.
- New examples, enough to give every chart a shape: 21 bugs (open, deferred,
  fixed, cancelled, two that came back), 16 plans including a second milestone,
  12 example backlog tasks, 14 features, 5 releases and 10 learnings, spread
  over June to October 2026.

### Changed

- Plan stages add `approved` and `deferred`; backlog task stages add `deferred`
  and `cancelled`, and a `done` task now requires `completed`; bug stages add
  `deferred`.
- `data/plans.md` lists open plans only (`## Active`, `## Deferred`), and
  `data/bugs.md` open tickets only (`## Open`, `## Deferred`). Milestone plans
  move a shipped child to a `## Shipped` section.
- A release `version` must be a semantic version or `unreleased`.
- The ship, plan, verify and weekly skills follow the archive moves, the bug
  fields, guard tests and learnings. `AGENTS.md`, `SCHEMA.md`, `STRUCTURE.md`
  and the start page describe them.
- The open bug example gained the new fields and an `## Expected` section; its
  suspected cause moved to `## Leads`.
- `STRUCTURE.md` no longer carries the notes on where the template came from; it
  describes the layout and its rationale only.

### Fixed

- Hub intros and example convention notes still said `status` where the field
  has been `stage` since 1.1.0.
- The codebase-map key example in `SCHEMA.md` now uses a fictional path.

### Migration

If you cloned 1.1.0, in your workspace:

1. Copy the changed schemas (`bug`, `plan`, `feature`, `task`, `release`, `hub`)
   and the new `learning.yaml`, plus every `*.ui.yaml`, into `.iwe/schemas/`.
2. Add the `[schemas.learning]` binding and the `data/learnings`,
   `data/plans-archive` and `data/bugs-archive` hub entries to
   `.iwe/config.toml`.
3. Add `created`, `reported_by` and `affected_versions` to every bug, an
   `## Expected` section where it is missing, and `completed` to fixed bugs and
   done tasks. Use `unreleased` when no version has shipped.
4. Add `tests` to every feature marked `implemented`.
5. Create the two archive hubs and `data/learnings.md`, move the links of closed
   plans and bugs into the archives, and list them in `data/index.md`.
6. Run `iwe normalize`, then `iwe schema validate`.

## [1.1.0]

### Added

- `index.md` — a start page at the root: what the workspace is, how to begin,
  and where things live. The iWe app opens it when it creates a workspace from
  this template.
- `preview.png` — the image the iWe template gallery shows for this template.
- `data/` is now a conformant [Open Knowledge
  Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  v0.2 bundle — portable knowledge any OKF consumer can read, not just iwe.
- **Every document carries a `type`.** Work items already had schemas; the
  reference documents and hubs did not, and typing them makes
  `iwe find --filter '{type: feature}'` possible for the first time.
- Six new schemas: `spec`, `architecture`, `concept`, and `someday` for the
  reference documents, plus `hub` for the directory landing pages and `tracker`
  for `data/product.md` and `data/milestone.md`.
- Three conformance schemas in `.iwe/schemas/` — `okf.yaml` (every document
  carries a non-empty `type`), `okf-index.yaml`, and `okf-log.yaml` (the body
  shapes of the reserved files). They stack on top of the per-type schemas, so a
  document is checked by both.
- OKF's optional families on every type: `description` (one sentence, on all 32
  documents), `generated` (who wrote it and when), `sources` (what it was
  derived from — a codebase map cites the code path it describes), `resource`,
  `stale_after` (set on codebase maps, which go stale when the code moves), and
  `tags`.
- `data/log.md` — a date-grouped history of the workspace. The `ship` skill
  appends to it on every release.
- A GitHub Actions workflow that runs `iwe schema validate` and checks that
  `iwe normalize` is a no-op on every push and pull request.
- `template.toml` — the manifest of the iWe template format (id, name,
  description, version, license), so the template can be listed in the [iWe
  template registry](https://github.com/iwe-org/templates) and created from the
  app with File ▸ New Workspace from Template…. `README.md` and `CHANGELOG.md`
  are excluded from the installed workspace.

### Changed

- **`CLAUDE.md` removed.** It only pointed at `AGENTS.md`, which Claude Code now
  reads directly.
- **`status` is now `stage`.** OKF reserves `status` for its lifecycle values
  (`draft | stable | deprecated`), so the workflow field moved to `stage` in all
  schemas and every document. `status` is now set only where it carries signal —
  the mapping is the table in `SCHEMA.md`, and the skills maintain both fields
  together.
- **Codebase maps use OKF's `verified`.** The date-valued `verified: 2026-07-25`
  became `verified: { by: <actor>, at: <ISO 8601> }`, which is what the field
  always meant — who last confirmed the doc against the code. `source` and
  `commit` are unchanged.
- **Reference documents are validated.** The "hub documents stay unvalidated by
  design" exemption is deliberately retired: OKF conformance requires a `type`
  everywhere. Reference types still carry no `stage` — nothing about them is a
  lifecycle.
- **Links carry `.md`.** `refs_extension` changed from `""` to `".md"` so links
  resolve for readers outside iwe. Run `iwe normalize` after changing it.
- **`data/index.md` is the bundle-root index.** Its only frontmatter is
  `okf_version: "0.2"`, and its body is sections of link bullets carrying each
  target's description.
- **Config version 4.** `.iwe/config.toml` moved from `version = 3` to
  `version = 4`, and the `[library]` table is now `[workspace]`. `iwe normalize`
  performs the migration; it also re-wrapped the paragraphs of ten example
  documents, with no change to their text.
- **CI runs `iwe` 0.32.0**, up from 0.19.0.
- `.gitignore` ignores `.iwe/app/`, the folder the iWe app now keeps its state
  in.

### Fixed

- A convention note in `data/codebase/timer.example.md` opened an emphasis span
  that wrapped a bullet list, which `iwe normalize` rewrote differently on
  alternating runs. Rewritten as a plain paragraph so normalization is stable.

### Migration

If you cloned an earlier copy of this template, in your workspace:

1. Rename `status:` to `stage:` in every document and schema.
2. Add `type:` to every document under `data/` — the type name matching its
   directory (`plan`, `feature`, `bug`, `release`, `task`, `codebase`, `spec`,
   `architecture`, `concept`, `someday`, `hub`, `tracker`).
3. Convert `verified:` on codebase maps from a date to `{ by, at }`.
4. Set `refs_extension = ".md"` in `.iwe/config.toml` and run `iwe normalize`
   from inside the workspace directory.
5. Copy the new schemas and their `[schemas.*]` bindings from this template into
   your `.iwe/`.
6. Add `description` and `generated` to your documents — the agent can backfill
   these in one pass.
7. Run `iwe normalize` with `iwe` 0.32.0 to migrate `.iwe/config.toml` to
   version 4, and add `.iwe/app/` to your `.gitignore`.

Checked with `iwe` 0.32.0, the version CI runs; an older build may not read a
version 4 config.
