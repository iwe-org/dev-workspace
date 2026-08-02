# Dev workspace

[![OKF
v0.2](https://img.shields.io/badge/OKF-v0.2%20conformant-blue)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)

**Memory for your coding agent.** A ready-to-use markdown knowledge base for a
software project: what the product is, how it must behave, what's planned,
shipped, broken, and released — structured as a queryable graph your agent reads
at the start of every session and writes back to before it ends.

Agents are good at writing code and bad at remembering why. Chat history
evaporates, and the reasons behind last month's design decision evaporate with
it. This workspace is the fix: a system of record the agent maintains as it
works, so every session starts with context instead of archaeology.

It is also a conformant [Open Knowledge
Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle — an open standard for agent-maintained knowledge — so your project
graph is portable to any OKF consumer, not locked to one tool. Every document
carries a `type`, provenance (`generated`, `sources`) and lifecycle (`status`,
`stale_after`) live in frontmatter, and conformance is checked in CI on every
commit.

## Quickstart

1. **Use this template** (GitHub → *Use this template*) or clone it — either
   standalone next to your code, or as a `docs/` folder inside your repo.

2. **Install [IWE](https://iwe.md)** — the CLI that gives the graph its queries,
   renames, and validation:

   ``` bash
   brew install iwe-org/iwe/iwe
   ```

3. **Open the workspace in your coding agent** (Claude Code, or anything that
   reads `AGENTS.md`) and say: **"run the setup"**. The setup skill reads your
   codebase, drafts the product doc and a starting architecture note from it,
   asks you only what the code can't answer, and deletes the example documents.

## How you use it

**Day one.** Run the setup. For an existing codebase it's mostly reading — the
skill drafts from your code and you correct; greenfield, it's a short interview.

**Every session after.** The loop is: **explore**
(`"let's think about dark mode"` — a thinking partner that reads code and graph
but never implements) → **plan** (`"plan dark mode"` — files a plan with
verified code anchors and the specs it will touch) → **implement**
(`"work on the next task"` — executes the plan task-by-task, ticking checkboxes
as tests pass) → **verify** (`"is it ready to ship?"` — checks tasks,
requirements, and scenarios against the actual code) → **ship**
(`"ship dark mode"` — syncs the specs, flips stages, records the work in the
unreleased page) → occasionally **weekly** (`"weekly digest"` — what moved,
what's stuck, what's next). Enter and leave the loop anywhere — small work can
skip straight from plan to ship. Each step leaves the graph consistent, so the
next session — or the next agent — picks up exactly where this one stopped.

Ask questions against the graph instead of re-reading the codebase:

``` bash
iwe find --filter '{stage: done}' --included-by data/plans -f keys  # what shipped
iwe find --references data/spec/timer -f keys                        # what depends on this spec
iwe retrieve -k data/plans/mvp --expand-includes 1                   # a milestone and its plans
iwe tree -k data/features                                            # the feature landscape
```

## What's inside

```
data/
├── index.md          # the graph root — every hub is a child
├── product.md        # what you're building — the doc every session reads first
├── plans.md    plans/         # implementation plans: Active / Done / Cancelled
├── features.md features/      # feature docs, proposed → implemented
├── bugs.md     bugs/          # symptom / repro / root cause / fix
├── spec.md     spec/          # behavioral specs — the durable truth, synced at ship time
├── architecture.md architecture/  # design decisions, with the rejected alternatives
├── concept.md  concept/       # product vision — the tiebreaker for feature debates
├── codebase.md codebase/      # the code map: a component tree + flows — derived, refreshable
├── releases.md releases/      # one page per version + the unreleased accumulator
├── backlog.md  backlog/       # prioritized next work
├── someday.md  someday/       # ideas that aren't commitments
└── milestone.md               # milestones: plans whose children are plans
```

Every document carries typed, schema-validated frontmatter —
`iwe schema validate` is the commit gate. Relationships are links, not fields;
the `*.example.md` docs show every convention on a small fictional app and are
deleted by setup.

`AGENTS.md` is the agent's operating manual. `SCHEMA.md` documents the
frontmatter shapes. `STRUCTURE.md` explains the design decisions behind the
layout.

## Built on IWE

[IWE](https://iwe.md) treats markdown files as a graph: a link on its own line
makes the target a child document, so hubs, milestones, and release pages are
structure, not convention. The CLI queries it (`iwe find`, `iwe tree`),
refactors it safely (`iwe rename` updates every reference), and validates it
(`iwe schema validate`). Editor support (VSCode, Neovim, Zed, Helix) via LSP.

## Acknowledgments

The spec-sync-at-ship discipline and the Requirement/Scenario spec format are
adapted from [OpenSpec](https://github.com/Fission-AI/OpenSpec)'s spec-driven
workflow. The workspace shape follows its sibling template,
[marketing-workspace](https://github.com/iwe-org/marketing-workspace).

## License

MIT — see `LICENSE.md`.
