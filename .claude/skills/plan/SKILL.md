---
name: plan
description: Create an implementation plan — discovery in the real codebase first, then a plan document with verified code anchors, spec impact, and dependencies, filed under Active in the plans hub. Use when the user says "plan <feature>", "write a plan for ...", or asks to turn a backlog task or accepted feature into work.
---

# Plan a piece of work

A plan is a promise a future session can execute without re-deriving context.
Discovery happens in the codebase before a word is written; every anchor is
verified, every touched spec is named.

## Steps

1. **Consult.** Read `data/product.md` — especially `## Constraints` and
   `## Authoring rules`, which bind what you write. Check for related work:
   `iwe find --fuzzy <topic> -f keys`, the relevant `data/spec/` and
   `data/features/` docs, and whether an active plan already covers this
   (`iwe find --included-by data/plans -f keys`).
2. **Discover.** Read the code the plan will touch. Collect the entry points,
   the functions to modify, and their current line numbers — these become
   `## Key references`, and they must come from the current checkout, not
   memory.
3. **Create.**
   `iwe new "<Title>" --key data/plans/<YYYYMMDD>-<slug> --content ""` (today's
   date, kebab slug; the title is required, and the empty `--content` keeps the
   command from waiting for piped input). It writes the `# <Title>` heading;
   add the frontmatter above it:

   ```yaml
   ---
   type: plan
   created: <today>
   ---
   ```

   Body sections, in order (omit a section only when it's genuinely empty):
   - `## Context` — why now, linking the feature/bug/backlog doc that
     motivates it
   - `## Approach` — the shape of the solution and the alternative you
     rejected, in a few sentences
   - `## Implementation Steps` — `### Task N: <name>` blocks, each with
     `**Files:** Create:/Modify: ...` and `- [ ]` checkboxes
   - `## Spec changes` — every `data/spec/` doc this work will create or
     change; name not-yet-existing specs in back-ticks (never dangling links)
   - `## Depends on` — inline links to plans that must ship first (omit if
     none)
   - `## Verification` — how a session proves the work is done: commands,
     tests, manual checks
   - `## Out of scope` — what this plan deliberately does not do
   - `## Key references` — `path:line — symbol` list under a line
     `Verified anchor points (line numbers as of <today>):`
4. **File it.** Add an inclusion link under `## Active` in `data/plans.md`, and
   under the phase it advances in its milestone plan if one schedules it,
   with `tags: [milestone-<slug>]` in the plan's frontmatter. If the plan
   implements a proposed feature, accept the feature and clear the draft status
   that `proposed` carried:
   `iwe update -k data/features/<slug> --set stage=accepted --unset status`. If
   it grew from a backlog task, mark the task done (`stage: done`, `completed`)
   and move its link to `## Done` in `data/backlog.md`. If it replaces an older
   plan, link the old plan from
   `## Context` and cancel it in the same change (the ship skill's cancel
   path).
5. **Validate.** `iwe normalize`, then `iwe schema validate` — must pass.

## Rules

- One plan per topic; if the plan needs two unrelated verification stories,
  it's two plans.
- Code anchors are verified against the current checkout and stamped with the
  date — never cite from memory.
- `## Spec changes` is mandatory thinking, even when its honest content is
  "none — no behavioral change".
- Scale ceremony with risk: a small low-risk plan can be short, but never
  skip Verification.
