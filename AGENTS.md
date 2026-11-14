# Agent operating manual

You are operating a **dev workspace**: a markdown knowledge graph that is a
software project's memory and system of record.

- **Code** lives in the project's own repository. Writing it is your normal work
  — this workspace doesn't change how you code.
- **Project state** lives here, in `data/` — what the product is, how it must
  behave, how it's designed, what's planned, shipped, broken, and released.
  Every working session must leave a record in the graph; this is your memory
  across sessions. Never keep project state only in conversation.

This manual holds what every session needs. The fields, stage values and body
shape of each document type are in `SCHEMA.md`, the workflows are the skills
listed at the end, and the reasons for the layout are in `STRUCTURE.md`. Read
those when the work calls for them.

## Start of every session

1. Read `data/product.md`. If `## What is it` is still an unfilled ✏️, the
   workspace has not been set up: run the setup skill before anything else. If
   only other sections still hold a ✏️, setup ran and the developer skipped
   them: name them once, then carry on. `## Constraints` and
   `## Authoring rules` bind everything you write.
2. Check the state of work: the plans under `## Active` in `data/plans.md`, and
   the high-priority tasks —
   `iwe find --filter '{stage: planned, priority: high}' --included-by data/backlog -f keys`.

## The operating loop

1. **Pick** the next piece of work: the user's request, an active plan, or the
   backlog head.

2. **Consult** before acting: the `data/spec/` and `data/architecture/` docs for
   the area, the feature or bug doc the work belongs to, and the learnings on it
   (`iwe find --lexical "<topic>" --included-by data/learnings -f keys`).

3. **Execute** in the codebase. Work that deserves a plan gets one first.

4. **Record** the state change, in the same session:

   - Work starts → plan skill. Being built → implement skill.
   - Work ships, a bug is fixed, a plan is dropped, a release is cut → verify
     skill, then ship skill. Changed behavior goes into its spec there.
   - A bug is found or reported → look for its ticket first, open or archived
     (`iwe find --lexical "<symptom>" --filter '{type: bug}' -f keys`); a bug
     that came back reopens its old ticket. Otherwise file `data/bugs/<slug>.md`
     **before** writing any fix (`SCHEMA.md`, Bugs).
   - Something learned the hard way — a pitfall, a standing rule the developer
     gave, a belief they corrected → `data/learnings/<slug>.md`. Only what the
     next session needs and the code, specs and tickets don't already say.
   - A design decision → `data/architecture/<slug>.md`, with the alternatives
     that were rejected. A vision insight → `data/concept/<slug>.md`.
   - An actionable item → `data/backlog/<slug>.md`. An idea that is not a
     commitment → `data/someday/<slug>.md`.

   A new document is linked from its hub (`data/<folder>.md`) in the same edit.

5. **Stamp** every document you create or meaningfully change:
   `generated: { by: <your agent>/<your model>, at: <ISO 8601 now> }`, naming
   the agent and model you are actually running as (for example
   `claude-code/opus-5`); a one-sentence `description` if it has none; a
   `sources` entry when you derived it from code or an external page. Whenever
   you set `stage`, set or clear `status` in the same edit, from the table in
   `SCHEMA.md`. If a hub was added or removed, update `data/index.md`.

6. **Validate** — `iwe normalize`, then `iwe schema validate` must pass. If the
   workspace is a git repository (`git rev-parse --is-inside-work-tree`), commit
   with a short message naming the state change. If it is not, there is nothing
   to commit, and you do not create a repository unasked.

## Conventions

- **Inclusion link** — a markdown link on its own line makes the target a child
  in the graph. A hub's inclusion link, not the directory, is what makes a
  document a plan or a feature. A link inside a sentence or a list item is a
  soft reference.
- **A stage lives in two places** — in frontmatter and as the position of the
  item's link in its hub. Change both in one edit, as part of the change that
  finishes the work. Frontmatter is the record: hubs, queries and charts never
  read the body, so a plan with every box ticked and no `stage: done` is still
  active. A closed item's link moves; it is never removed.
- **Links** are markdown links, never wiki links, relative to the file that
  contains them and ending in `.md` (`[Timer](spec/timer.md)`).
- **One topic per file**, named as `SCHEMA.md` shows for its type.
- **`data/` is an OKF v0.2 bundle**: every document carries a `type`, and
  `data/index.md` and `data/log.md` keep their reserved shapes (`SCHEMA.md`,
  Reserved files). Never get past a validation failure by unbinding a schema or
  loosening one.
- **Git is optional.** Where a skill says commit, it means: if the workspace is
  a git repository. Without one, frontmatter dates and `generated.at` are the
  history.
- **Example docs** — files ending in `-example.md`, and the releases named
  `example-<version>.md`, show each document shape on a fictional product.
  Ignore them when reporting real project state. The setup skill deletes them,
  and this paragraph, at the end of onboarding.

## IWE basics

The graph is managed by the `iwe` CLI ([IWE](https://iwe.md)), run from the
workspace root.

- A document's **key** is its path without the extension (`data/product`,
  `data/plans/20260801-dark-mode`). Its title is its H1.
- **Never `mv` or hand-delete a document.** `iwe rename` and `iwe delete` update
  every link in the graph and say how many documents they changed; those edits
  are part of the change.
- Run `iwe normalize` after any manual edit instead of wrapping text or fixing
  link targets by hand.
- The `iwe` skill has the commands and the query syntax; read it before a query
  or a restructuring you have not run this session.
- If `iwe` is not installed, the workspace is still plain markdown you can read
  and edit, but queries, renames and validation need it. Ask the user to install
  it (`brew install iwe-org/iwe/iwe` or `cargo install iwe`; see
  https://iwe.md/quick-start/) before restructuring anything.

## Workspace skills

| Skill                               | What it does                                                            |
| ----------------------------------- | ----------------------------------------------------------------------- |
| `.claude/skills/setup/SKILL.md`     | Onboarding: scans the codebase, drafts product and architecture docs    |
| `.claude/skills/explore/SKILL.md`   | Thinking partner: investigate and compare options; never writes code    |
| `.claude/skills/plan/SKILL.md`      | Files a plan: discovery, verified anchors, spec impact, Active listing  |
| `.claude/skills/implement/SKILL.md` | Executes a plan task-by-task: tests, checkbox ticks, clean boundaries   |
| `.claude/skills/verify/SKILL.md`    | Pre-ship gate + drift audit: claims in the graph checked against code   |
| `.claude/skills/ship/SKILL.md`      | Closes the loop: spec sync, stage flips, release recording, release cut |
| `.claude/skills/weekly/SKILL.md`    | Read-only digest: shipped, in flight, bugs, backlog, graph health       |
| `.claude/skills/iwe/SKILL.md`       | The `iwe` CLI reference: reading, querying, restructuring, validating   |
