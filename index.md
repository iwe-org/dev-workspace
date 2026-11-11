# Dev Workspace

The memory of a software project, kept by your coding agent: what the product
is, how it must behave, what is planned, shipped, broken, and released. The
agent reads it at the start of every session and writes back to it before the
session ends, so the next one starts with context instead of archaeology.

## Start here

1. Ask the agent to **run the setup**. It reads your codebase, drafts the
   [product doc](data/product.md) and a first architecture note from what the
   code shows, asks you only what the code can't answer, and removes the example
   documents.
2. Then work in the loop. **Explore** an idea with the agent as a thinking
   partner, **plan** it, **implement** the plan task by task, **verify** it
   against the code, and **ship** it. Each step is a skill the agent runs, and
   each leaves the graph consistent for the next session.
3. Once a week, ask for the **weekly digest**: what moved, what is stuck, and
   what comes next.

## Where things live

- [Product](data/product.md) — what you are building; every session reads it
  first.
- [Plans](data/plans.md) — one plan per unit of work: active, done, and
  cancelled.
- [Backlog](data/backlog.md) — work items waiting to become a plan or a quick
  fix.
- [Features](data/features.md) — feature documents, from proposed to
  implemented.
- [Bugs](data/bugs.md) — symptom, reproduction, root cause, and fix.
- [Spec](data/spec.md) — how the product must behave, synced when work ships.
- [Architecture](data/architecture.md) — design decisions and the alternatives
  that were rejected.
- [Releases](data/releases.md) — one page per version and what it shipped.

The [graph root](data/index.md) links every hub. [Structure](STRUCTURE.md)
explains the layout, [Schema](SCHEMA.md) lists the frontmatter each kind of
document carries, and the [agent manual](AGENTS.md) is what the agent follows.
