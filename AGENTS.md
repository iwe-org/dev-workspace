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
   `iwe find --filter '{status: planned, priority: high}' --included-by data/backlog -f keys`.

## The operating loop

1. **Pick** the next piece of work (the user's request, an active plan, or the
   backlog head).
2. **Consult** before acting: the relevant `data/spec/` docs (intended
   behavior), `data/architecture/` (design and past decisions), and the
   feature/bug doc the work belongs to. If a plan exists, execute the plan; if
   the work deserves one, write it first (plan skill).
3. **Execute** — implement in the codebase, following the plan's tasks (the
   implement skill keeps checkboxes, anchors, and deviations honest while you
   do).
4. **Record** — write the state back:
   - Idea (not a commitment) → `data/someday/<slug>.md` + link from
     `data/someday.md`.
   - Actionable item → `data/backlog/<slug>.md` (`status: planned`, priority),
     linked under the priority section of `data/backlog.md`.
   - Work starts → plan skill: `data/plans/YYYYMMDD-<slug>.md` (`created`,
     verified code anchors, `## Spec changes`) + link under `## Active`.
   - Work ships → verify skill green (tasks, requirements, and scenarios checked
     against the code), then ship skill: specs synced first, then `status: done`
     with `completed`, link moved to `## Done`, feature doc `implemented`,
     inclusion link in `data/releases/unreleased.md`.
   - Plan abandoned → `status: cancelled`, link moved to `## Cancelled` (it
     stays listed — the record of why is worth keeping).
   - Bug found → `data/bugs/<slug>.md` (Symptom / Reproduction / Root cause /
     Fix, `path:line` anchors) + link from `data/bugs.md`. Fixed →
     `status: done`.
   - Behavior defined or changed → the matching `data/spec/` doc
     (Requirement/Scenario format); this happens *inside* the ship flow, not as
     an afterthought.
   - Design decision made → `data/architecture/<slug>.md`, including the
     rejected alternatives.
   - Code structure changed (module added, split, or moved) → refresh the
     touched `data/codebase/` docs (the map skill's refresh mode — the skill
     ships in the [code-map template](https://github.com/iwe-org/code-map)).
   - Vision insight → `data/concept/<slug>.md`.
   - Task finished → `status: done` + `completed` on the task doc, link moved to
     `## Done` in `data/backlog.md`.
   - Release cut → ship skill's release mode (rename unreleased, stamp
     version/date, fresh accumulator).
5. **Validate & commit** — `iwe normalize`, then `iwe schema validate` must
   pass; commit with a short message describing the state change.

## Conventions

- **Inclusion link** = a markdown link on its own line — it makes the target a
  child in the graph. Hubs (`data/plans.md`, `data/features.md`, …)
  inclusion-link their members; that link, not the directory, is what makes a
  document a plan or a feature. Inline links (inside sentences/list items) are
  soft references for cross-cutting relationships.
- **Dual representation**: a work item's status lives in frontmatter *and* as
  its link's position in the hub (`## Active`/`## Done`/`## Cancelled` in plans,
  `## High`/`## Done` in backlog). Change both together; every item stays listed
  forever.
- **Status vocabularies** (schema-enforced, human reference in `SCHEMA.md`):
  plans `done|cancelled` (absent = active, `done` requires `completed`);
  features `proposed|accepted|implemented|deprecated|cancelled`; bugs
  `done|cancelled` (absent = open); releases `released|unreleased`; backlog
  `planned|done`. Reference docs (spec/architecture/concept/someday) carry no
  frontmatter; codebase-map docs carry `source` + `commit` + `verified` —
  provenance, not lifecycle.
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
- **Example docs**: files suffixed `.example.md` demonstrate each directory's
  document shape (a fictional product; schema-validated so they can't rot).
  Ignore them when reporting real project state; the setup skill deletes them at
  the end of onboarding.
- **Frontmatter shapes are enforced** — `.iwe/schemas/*.yaml` is the validation
  gate, bound to key globs in `.iwe/config.toml`.

## iwe basics

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

## iwe CLI cheatsheet

``` bash
iwe retrieve -k data/spec/timer                                  # read a doc
iwe retrieve -k data/plans --expand-includes 1                   # hub + children
iwe find --fuzzy timer -f keys                                   # fuzzy title+key match
iwe find --lexical "session log storage" -f keys                 # full-text ranking
iwe find --included-by data/plans -f keys                        # all docs under a hub
iwe find --references data/spec/timer -f keys                    # backlinks
iwe find --filter '{status: done}' --included-by data/plans -f keys   # frontmatter query
iwe tree -k data/plans -d 2                                      # subtree overview
iwe new --key data/plans/20260801-my-plan                        # create at an explicit key
iwe update -k data/features/foo --set status=implemented         # set frontmatter
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

| Skill                                                    | What it does                                                              |
| -------------------------------------------------------- | ------------------------------------------------------------------------- |
| `.claude/skills/setup/SKILL.md`                          | Brownfield onboarding: scans the codebase, drafts product/architecture    |
| [code-map template](https://github.com/iwe-org/code-map) | Map + benchmark skills for `data/codebase/` — copy them in from that repo |
| `.claude/skills/explore/SKILL.md`                        | Thinking partner: investigate and compare options; never writes code      |
| `.claude/skills/plan/SKILL.md`                           | Files a plan: discovery, verified anchors, spec impact, Active listing    |
| `.claude/skills/implement/SKILL.md`                      | Executes a plan task-by-task: tests, checkbox ticks, clean boundaries     |
| `.claude/skills/verify/SKILL.md`                         | Pre-ship gate + drift audit: claims in the graph checked against code     |
| `.claude/skills/ship/SKILL.md`                           | Closes the loop: spec sync, status flips, release recording, release cut  |
| `.claude/skills/weekly/SKILL.md`                         | Read-only digest: shipped, in flight, bugs, backlog, graph health         |
