---
name: verify
description: Verify that implementation matches the graph — check a plan's tasks, spec requirements, and scenarios against the actual code before shipping, or sweep the whole workspace for doc↔code drift. Use when the user says "verify <plan>", "is this ready to ship", "audit the docs", or "check for drift".
---

# Verify implementation against the graph

The graph makes claims — tasks checked, requirements SHALL-ed, features marked
implemented. This skill tests those claims against the code and reports what
doesn't hold. It never fixes anything: the report is the deliverable, and the
fixes belong to the skills and sessions it points at.

## Steps

1. **Pick the mode.** A plan named or inferable from conversation → plan mode.
   No plan ("audit the docs", "check for drift") → audit mode (step 5).
2. **Select the plan.** If ambiguous, list the plans under `## Active` in
   `data/plans.md` (a just-finished plan may also sit under `## Done`) and ask.
   Announce: "Verifying: <plan>". Read the full plan, every spec named in its
   `## Spec changes`, and the linked feature/bug docs
   (`iwe retrieve -k data/plans/<key>`).
3. **Check three dimensions**, collecting issues as CRITICAL / WARNING /
   SUGGESTION, each with a concrete recommendation:
   - **Completeness** — every unchecked `- [ ]` task is a CRITICAL
     ("complete it, or tick it if already done"). Every spec in
     `## Spec changes` exists and reflects the change (a back-ticked
     not-yet-created spec still pending at verify time is a CRITICAL). Every
     `### Requirement:` in the touched specs has implementation evidence in
     the codebase — search for it; none found is a CRITICAL.
   - **Correctness** — map each requirement to `path:line` evidence and judge
     whether the implementation matches its SHALL statement (divergence is a
     WARNING with the file and lines to review). For each `#### Scenario:`,
     check the condition is handled in code and covered by a test (uncovered
     scenario is a WARNING). Run the plan's `## Verification` commands and
     report their actual output — a failing command is a CRITICAL.
   - **Coherence** — the implementation follows the plan's `## Approach` (a
     different approach that works is a WARNING: either the code or the plan
     should change); `## Out of scope` items stayed out; the
     `## Authoring rules` in `data/product.md` were honored.
4. **Report and stop.** Issues ranked most severe first, then the verdict:
   **ready to ship** (zero CRITICAL — hand off to the ship skill) or the
   blocker list. Do not fix, tick, or edit anything.
5. **Audit mode** — the same discipline over the whole graph, against the
   codebase:
   - Specs whose requirements the code now contradicts (sample the
     highest-traffic specs first: `iwe find --references <key>` counts).
   - Features marked `implemented` with no trace in the code; shipped
     behavior with no feature doc.
   - Open bugs (no `stage`) — still reproducible? Point at ones whose
     `## Key references` no longer exist.
   - Stale map docs: for each `data/codebase/` doc, commits touching its
     `source` after its `commit`
     (`git log --oneline <commit>..HEAD -- <source>`) — flag for the map
     skill's refresh mode.
   - Consistency: plans with `stage: done` still linked under `## Active`
     (`iwe find --filter '{stage: done}' --included-by data/plans -f keys`
     cross-checked against the hub sections); `iwe schema validate`
     violations; `iwe stats` dangling links and `data/` orphans other than
     `data/index`.
   - Report in the same CRITICAL/WARNING/SUGGESTION format, grouped by fix
     owner: "run the ship skill on X", "update spec Y", "close bug Z".

## Rules

- Report, never fix — even a one-character hub-section fix is someone else's
  commit, so the audit trail stays clean.
- Every claim cites `path:line` evidence or is explicitly labeled
  "unverified" — a requirement you couldn't trace is unverified, not failed,
  and says so.
- Run only the verification commands the plan itself names (tests, builds);
  anything with side effects beyond the working tree needs the user's go-ahead.
- Ship follows a clean verify; a verify with CRITICALs ends with the blocker
  list, not a softened verdict.
