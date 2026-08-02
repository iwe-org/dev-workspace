---
name: ship
description: Close the loop when work ships — sync the specs the plan touched, mark the plan done, flip the feature doc to implemented, and link the work into the unreleased page; also cuts a release when asked. Use when the user says "ship <plan>", "mark <plan> done", "the <feature> work is finished", or "cut release X.Y.Z".
---

# Ship finished work

Shipping is a state change across the whole graph, not one frontmatter flip.
Specs sync first — a plan is not done while the specs it invalidated still
describe the old behavior.

## Steps

1. **Confirm.** Identify the plan and check its `## Verification` section
   against reality (test output, the diff, a manual check) — the verify skill
   (`.claude/skills/verify/SKILL.md`) is the thorough form of this step, and a
   clean verify is the best possible input to shipping. If verification can't
   be confirmed, stop and say what's missing — don't mark done on hope.
2. **Sync specs.** Walk the plan's `## Spec changes` list. For each entry,
   update the `data/spec/` doc to describe the behavior that actually shipped
   (create the doc if it doesn't exist yet, link it from `data/spec.md`, and
   replace the back-ticked name in the plan with a real link). Requirement /
   Scenario format; update scenarios that the change falsified.
3. **Flip the plan.**
   `iwe update -k data/plans/<key> --set status=done --set completed=<today>`,
   then move its link in `data/plans.md` from `## Active` to `## Done`.
   (Abandoned instead of shipped? `--set status=cancelled`, move to
   `## Cancelled`, and stop here.)
4. **Update the feature.** Set the feature doc to `status: implemented`
   (create it from the plan's Context if it doesn't exist, linked from
   `data/features.md`). Fixed a bug instead? Set the bug doc to
   `status: done`.
5. **Record in the release.** Add an inclusion link to the feature doc under
   `## Added` in `data/releases/unreleased.md` (bug fixes under `## Fixed`,
   linking the bug doc).
6. **Cut a release** (only when asked, e.g. "cut release 0.2.0"):
   - `iwe rename data/releases/unreleased data/releases/<X.Y.Z>`
   - `iwe update -k data/releases/<X.Y.Z> --set version=<X.Y.Z> --set date=<today> --set status=released`
   - Recreate `data/releases/unreleased.md` fresh (version/status
     `unreleased`, empty `## Added` / `## Fixed`)
   - In `data/releases.md`, keep `Unreleased` on top and add the new version
     right below it (newest first).
7. **Validate & commit.** `iwe normalize`, then `iwe schema validate` — must
   pass. Commit with a message describing the state change, e.g.
   `ship: focus-sessions done, timer spec synced, 0.2.0 cut`.

## Rules

- Specs sync before the status flips — never the other way around.
- A plan whose milestone aggregator lists it: check whether it was the last
  child; if so, tell the user the milestone is complete.
- Never invent verification results — if tests weren't run, say so and run
  them or ask.
- Release pages link feature and bug docs, not plans.
