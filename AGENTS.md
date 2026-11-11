# Agent operating manual

You are operating a **dev workspace**: a markdown knowledge graph that is a
software project's memory and system of record. The division of labor:

- **Code** lives in the project's own repository. Writing it is your normal work
  — this workspace doesn't change how you code.
- **Project state** lives here, in `data/` — what the product is, how it must
  behave (specs), how it's designed (architecture), what's planned, shipped,
  broken, and released. Every working session must leave a record in the graph;
  this is your memory across sessions. Never keep project state only in
  conversation.

## Start of every session

1. Read `data/product.md`. If it still contains ✏️ placeholders, run the setup
   flow (`.claude/skills/setup/SKILL.md`) before anything else — planning
   without product context is guessing. Note `## Constraints` and
   `## Authoring rules`: they bind everything you write.
2. Check the state of work: active plans under `## Active` in `data/plans.md`,
   and high-priority tasks —
   `iwe find --filter '{stage: planned, priority: high}' --included-by data/backlog -f keys`.

## The operating loop

1. **Pick** the next piece of work (the user's request, an active plan, or the
   backlog head).
2. **Consult** before acting: the relevant `data/spec/` docs (intended
   behavior), `data/architecture/` (design and past decisions), the feature/bug
   doc the work belongs to, and any learnings on the area
   (`iwe find --lexical "<topic>" --included-by data/learnings -f keys`). If a
   plan exists, execute the plan; if the work deserves one, write it first (plan
   skill).
3. **Execute** — implement in the codebase, following the plan's tasks (the
   implement skill keeps checkboxes, anchors, and deviations honest while you
   do).
4. **Record** — write the state back:
   - Idea (not a commitment) → `data/someday/<slug>.md` + link from
     `data/someday.md`.
   - Actionable item → `data/backlog/<slug>.md` (`stage: planned`, priority),
     linked under the priority section of `data/backlog.md`.
   - Work starts → plan skill: `data/plans/YYYYMMDD-<slug>.md` (`created`,
     verified code anchors, `## Spec changes`) + link under `## Active`.
   - Work ships → verify skill green (tasks, requirements, and scenarios checked
     against the code), then ship skill: specs synced first, then `stage: done`
     with `completed`, link moved to `data/plans-archive.md` (and to
     `## Shipped` in its milestone), feature doc `implemented` with its guard
     `tests`, inclusion link in `data/releases/unreleased.md`.
   - Plan abandoned or superseded → `stage: cancelled`, a first line in its body
     pointing to its successor or giving the reason, link moved to
     `## Cancelled` in `data/plans-archive.md` (it stays listed — the record of
     why is worth keeping).
   - Bug found or reported → search for an existing ticket first, open or
     archived (`iwe find --lexical "<symptom>" --filter '{type: bug}' -f keys`);
     a returning bug reopens its old ticket (SCHEMA.md, Bugs). Otherwise file
     `data/bugs/<slug>.md` **before** writing any fix, with `created`,
     `reported_by` and `affected_versions`, and the Symptom / Expected /
     Reproduction sections, + link under `## Open` in `data/bugs.md`. Fixed →
     `## Fix` written, `stage: done` + `completed`, link moved to `## Fixed` in
     `data/bugs-archive.md`.
   - Something learned the hard way — a pitfall you ran into, a standing rule
     the developer gave, a belief they corrected → `data/learnings/<slug>.md`
     (one fact, `created`, `kind`) + link from `data/learnings.md`. Only what
     the next session needs and the code, specs and tickets don't already say.
   - Behavior defined or changed → the matching `data/spec/` doc
     (Requirement/Scenario format); this happens *inside* the ship flow, not as
     an afterthought.
   - Design decision made → `data/architecture/<slug>.md`, including the
     rejected alternatives.
   - Vision insight → `data/concept/<slug>.md`.
   - Task finished → `stage: done` + `completed` on the task doc, link moved to
     `## Done` in `data/backlog.md` (`deferred` and `cancelled` tasks move to
     their own sections).
   - Release cut → ship skill's release mode (rename unreleased, stamp
     version/date, fresh accumulator).
5. **Stamp** — every document you create or meaningfully change gets
   `generated: { by: claude-code/opus-5, at: <ISO 8601 now> }`, a one-sentence
   `description` if it has none, and — when you derived it from code or an
   external page — a `sources` entry naming that path or URL. Whenever you set
   `stage`, derive OKF `status` from the table in `SCHEMA.md` and set or clear
   it in the same edit. If a hub gained or lost a document, update
   `data/index.md`.
6. **Validate & commit** — `iwe normalize`, then `iwe schema validate` must
   pass; commit with a short message describing the state change.

## Conventions

- **Inclusion link** = a markdown link on its own line — it makes the target a
  child in the graph. Hubs (`data/plans.md`, `data/features.md`, …)
  inclusion-link their members; that link, not the directory, is what makes a
  document a plan or a feature. Inline links (inside sentences/list items) are
  soft references for cross-cutting relationships.
- **Dual representation**: a work item's stage lives in frontmatter *and* as its
  link's position (`## Active`/`## Deferred` in `data/plans.md`, `## Done`/
  `## Cancelled` in `data/plans-archive.md`; `## Open`/`## Deferred` in
  `data/bugs.md`, `## Fixed`/`## Cancelled` in `data/bugs-archive.md`;
  `## High`…`## Cancelled` in backlog). Change both together; every item stays
  listed forever — a closed item's link goes to the archive hub.
- **Frontmatter is the record.** Only `stage: done` marks a plan as shipped;
  hubs and queries ignore the body, so ticked boxes or a closing remark leave it
  looking active. Set the stage in the same commit as the last piece of work,
  not afterwards.
- **Stage vocabularies** (schema-enforced, human reference in `SCHEMA.md`):
  plans `approved|deferred|done|cancelled` (absent = active, `done` requires
  `completed`); features `proposed|accepted|implemented|deprecated|cancelled`
  (`implemented` requires `tests`); bugs `deferred|done|cancelled` (absent =
  open, `done` requires `completed`); releases `released|unreleased`; backlog
  `planned|deferred|done|cancelled` (`done` requires `completed`). Reference
  docs (spec/architecture/concept/someday) and learnings carry a `type` and no
  stage.
- **`data/` is an OKF v0.2 bundle** — the graph is portable knowledge any [Open
  Knowledge
  Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  consumer can read, and CI checks conformance on every commit. Three rules keep
  it true: every document under `data/` has frontmatter with a non-empty `type`;
  `data/index.md` carries no frontmatter beyond `okf_version` and stays sections
  of link bullets; `data/log.md` stays date-grouped bullets under
  `## YYYY-MM-DD`. Each folder's schema requires `type`, and `okf-index.yaml`
  and `okf-log.yaml` check the reserved files — never work around them by
  unbinding a schema, and bind a new folder under `data/` to a schema of its
  own.
- **Links carry `.md`** — `refs_extension = ".md"`, so a link resolves for
  readers outside iwe. Run `iwe normalize` after bulk edits rather than
  hand-writing link targets.
- **Specs are the durable truth** and use `### Requirement:` + SHALL +
  `#### Scenario:` WHEN/THEN. The ship skill syncs them whenever a plan ships —
  a plan is not done while the specs it touched describe the old behavior. Scale
  rigor with risk: low-risk behavior gets two lines, contract behavior gets full
  scenarios.
- **Code anchors**: `path:line — symbol` lists under `## Key references`,
  stamped `Verified anchor points (line numbers as of YYYY-MM-DD):` — always
  from the current checkout, never from memory.
- **Naming**: plans are `YYYYMMDD-<kebab-slug>`; everything else is a short
  kebab slug; releases are `<semver>` plus `unreleased`. One topic per file.
- **Markdown links only, never wiki links.** References are extension-less
  (`[Timer](spec/timer)`), relative to the containing file.
- **Example docs**: files suffixed `-example.md`, and the releases named
  `example-<version>.md`, demonstrate each directory's document shape (a
  fictional product; schema-validated so they can't rot). Ignore them when
  reporting real project state; the setup skill deletes them at the end of
  onboarding.
- **Charts**: hubs carry ```` ``` chart ```` blocks — Vega-Lite specs in YAML
  whose `data` is a query (`{query: {filter, project}}`) or a data file beside
  the docs (`{url: releases/metrics.csv}`). They select documents by `type` and
  frontmatter fields, never by hub links, so they stay current as long as the
  frontmatter is right; never paste rows into a chart (inline `values` are
  refused). When you add a field or enum value a chart groups by, check the
  chart's `sort` and color `domain` lists in the same change.
- **Milestone tags**: a plan a milestone schedules carries
  `tags: [milestone-<slug>]` as well as its link in the milestone plan; the
  progress chart in `data/milestone.md` counts the tags.
- **Frontmatter shapes are enforced** — `.iwe/schemas/*.yaml` is the validation
  gate, bound to key globs in `.iwe/config.toml`. Each `<name>.ui.yaml` beside a
  schema only decorates it for the editor (titles, colors, hub columns); when
  you add a field or enum value to a schema, decorate it there too.

## IWE basics

The graph is managed by [IWE](https://iwe.md) — the `iwe` CLI. What you must
know:

- A document's **key** is its extension-less path relative to the repo root
  (`data/product`, `data/plans/20260801-dark-mode`) — that's what `-k` and the
  structural flags take.
- A document's title resolves from its H1 header.
- **Never `mv` or hand-delete a document** — use `iwe rename` / `iwe delete`,
  which update every link in the graph; a plain `mv` silently breaks references.
  After a rename or delete, check `git diff`: the reference updates are part of
  the change.
- Run `iwe normalize` after any manual edit — it keeps formatting, links, and
  structure consistent.
- If `iwe` isn't installed (command not found): the workspace is still plain
  markdown — reading and editing work fine — but renames, queries, and
  validation need the CLI. Ask the user to install it
  (https://iwe.md/quick-start/) before restructuring anything.

## IWE CLI cheatsheet

``` bash
iwe retrieve -k data/spec/timer                                  # read a doc
iwe retrieve -k data/plans --expand-includes 1                   # hub + children
iwe find --fuzzy timer -f keys                                   # fuzzy title+key match
iwe find --lexical "session log storage" -f keys                 # full-text ranking
iwe find --included-by data/plans -f keys                        # all docs under a hub
iwe find --references data/spec/timer -f keys                    # backlinks
iwe find --filter '{stage: done}' --included-by data/plans-archive:0 -f keys  # frontmatter query
iwe tree -k data/plans -d 2                                      # subtree overview
iwe new --key data/plans/20260801-my-plan                        # create at an explicit key
iwe update -k data/features/foo --set stage=implemented         # set frontmatter
iwe rename <old-key> <new-key>                                   # move; references auto-update
iwe delete <key>                                                 # delete + reference cleanup
iwe normalize                                                    # run after manual edits
iwe schema validate                                              # the commit gate (exit 0 = clean)
iwe stats                                                        # counts, orphans, broken links
```

Filters are YAML (`$eq`, `$ne`, `$in`, `$gte`, `$exists`, …), not jq. Structural
anchors: `--includes`, `--included-by`, `--references`, `--referenced-by`,
`--roots`.

## Workspace skills

| Skill                               | What it does                                                            |
| ----------------------------------- | ----------------------------------------------------------------------- |
| `.claude/skills/setup/SKILL.md`     | Brownfield onboarding: scans the codebase, drafts product/architecture  |
| `.claude/skills/explore/SKILL.md`   | Thinking partner: investigate and compare options; never writes code    |
| `.claude/skills/plan/SKILL.md`      | Files a plan: discovery, verified anchors, spec impact, Active listing  |
| `.claude/skills/implement/SKILL.md` | Executes a plan task-by-task: tests, checkbox ticks, clean boundaries   |
| `.claude/skills/verify/SKILL.md`    | Pre-ship gate + drift audit: claims in the graph checked against code   |
| `.claude/skills/ship/SKILL.md`      | Closes the loop: spec sync, stage flips, release recording, release cut |
| `.claude/skills/weekly/SKILL.md`    | Read-only digest: shipped, in flight, bugs, backlog, graph health       |
