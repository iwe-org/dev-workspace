---
name: implement
description: Execute an active plan task-by-task with state discipline — verified anchors, tests before checkbox ticks, clean stopping points, deviations written back into the plan. Use when the user says "implement <plan>", "continue the plan", "work on the next task", or asks to start building something that already has a plan.
---

# Implement a plan

Coding happens as it always does; this skill adds the discipline that keeps
the plan document true while it happens. The plan is the source of truth: the
code follows it, and where reality wins an argument, the plan is updated —
never silently outgrown.

## Steps

1. **Select the plan.** Infer from conversation; if ambiguous, list the plans
   under `## Active` in `data/plans.md` and ask (most recently created first).
   Announce: "Working on: <plan> — task N of M". No plan exists for what the
   user wants built? Stop and point at the plan skill.
2. **Load the context.** Read the full plan, the specs in its
   `## Spec changes`, and `data/product.md` `## Constraints` and
   `## Authoring rules` — they bind the code you're about to write.
3. **Check `## Depends on`.** Every prerequisite plan must carry
   `status: done`; if one doesn't, say so and stop — building on an unshipped
   dependency is how two plans end up half-true.
4. **Re-verify the anchors.** The plan's `## Key references` line numbers were
   true when written; re-locate each symbol in the current checkout before
   relying on it. If lines moved, update the anchor list and its
   "verified as of" date — that edit is part of this session's work.
5. **Execute the next unchecked task.** Follow its `**Files:**` list; write
   the code; run the tests that cover this task. When the task's work is done
   and its tests pass, tick its `- [ ]` → `- [x]` in the plan doc and
   `iwe normalize`.
6. **Write back deviations.** If implementation reveals the `## Approach` (or
   a task breakdown) is wrong, update the plan — amend the section, add or
   split tasks — *before* coding past the discrepancy, and tell the user what
   changed and why.
7. **Stop at a clean boundary.** After each task (or more, if the user asked
   for a longer run), report progress — "3/5 tasks, next: <task name>" — and
   stop. Blocked mid-task? Leave the box unchecked, describe the blocker, and
   stop there instead.
8. **Finish.** When the last box ticks, run the plan's `## Verification`
   commands, report the results, and suggest the verify skill for the full
   pre-ship check, then the ship skill.

## Rules

- Never tick a box whose tests fail or weren't run — an unchecked box that's
  actually done is a nuisance; a checked box that isn't done is a lie the next
  session builds on.
- Checkbox flips and anchor updates belong in the same commit as the code they
  describe.
- One task at a time unless the user asks for more; small honest increments
  beat a big unreviewable one.
- This skill implements; it doesn't ship. Status flips, spec sync, and release
  recording stay with the ship skill.
