# Dev Workspace

The memory of a software project, kept by your coding agent: what the product
is, how it must behave, what is planned, shipped, broken, and released. The
agent reads it at the start of every session and writes back to it before the
session ends, so the next one starts with context instead of archaeology.

## Start here

1. Install the `iwe` command-line tool. The agent's skills run it to query the
   graph, rename and delete documents without breaking links, and validate every
   change; without it the workspace is still readable markdown, but those steps
   fail.

   ``` bash
   brew install iwe-org/iwe/iwe   # or: cargo install iwe
   ```

   Other ways to install it are on the [IWE quick
   start](https://iwe.md/quick-start/).

2. Ask the agent to **run the setup**. It reads your codebase, drafts the
   [product doc](data/product.md) and a first architecture note from what the
   code shows, asks you only what the code can't answer, and removes the example
   documents.

3. Then work in the loop. **Explore** an idea with the agent as a thinking
   partner, **plan** it, **implement** the plan task by task, **verify** it
   against the code, and **ship** it. Each step is a skill the agent runs, and
   each leaves the graph consistent for the next session.

4. Once a week, ask for the **weekly digest**: what moved, what is stuck, and
   what comes next.

## Where things live

- [Product](data/product.md) — what you are building; every session reads it
  first.
- [Plans](data/plans.md) — one plan per unit of work; shipped and cancelled ones
  move to the [plans archive](data/plans-archive.md).
- [Milestones](data/milestone.md) — plans whose children are plans, one per
  release or goal.
- [Backlog](data/backlog.md) — work items waiting to become a plan or a quick
  fix.
- [Someday](data/someday.md) — ideas that are not commitments yet.
- [Features](data/features.md) — feature documents, from proposed to
  implemented.
- [Bugs](data/bugs.md) — one ticket per problem, opened before any fix: symptom,
  expected behaviour, reproduction, and fix. Closed tickets move to the [bugs
  archive](data/bugs-archive.md).
- [Releases](data/releases.md) — one page per version and what it shipped.
- [Spec](data/spec.md) — how the product must behave, synced when work ships.
- [Architecture](data/architecture.md) — design decisions and the alternatives
  that were rejected.
- [Concept](data/concept.md) — why the product exists, and the principles that
  settle a feature debate.
- [Learnings](data/learnings.md) — pitfalls, standing rules, and mistaken
  beliefs put right, one fact per page.
- [Log](data/log.md) — the dated history of what shipped and what was released.

The [graph root](data/index.md) links every hub. [Structure](STRUCTURE.md)
explains the layout, [Schema](SCHEMA.md) lists the frontmatter each kind of
document carries, and the [agent manual](AGENTS.md) is what the agent follows.
