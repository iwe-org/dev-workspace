---
name: weekly
description: Compose a weekly project digest from the workspace — what shipped, active plans and their staleness, open bugs, top backlog, and graph health, ending with the single most leveraged next action. Read-only; prints without writing files. Use when the user asks for a "weekly digest", "status report", or a start-of-week review.
---

# Weekly digest

A read-only sweep of the graph that answers: what moved, what's stuck, what's
next. Numbers come from frontmatter and git — never from memory.

## Steps

1. **Shipped.** Plans completed in the last 7 days:
   `iwe find --filter '{stage: done}' --included-by data/plans -f json` and
   keep those with a `completed` date in range; cross-check with
   `git log --since '7 days ago' --oneline`. List releases cut, if any.
2. **In flight.** Active plans (no `stage`) under `## Active` in
   `data/plans.md`; flag any untouched for 14+ days (last commit touching the
   file) as possibly stalled or silently done.
3. **Open bugs.** Docs under `data/bugs.md` without `stage: done/cancelled`.
4. **Backlog head.** `## High` items in `data/backlog.md`, plus how long
   they've sat (`created`).
5. **Graph health.** `iwe schema validate` (report violations) and `iwe stats`
   (dangling links, plus any `data/` document listed under Orphans other than
   `data/index` — every content doc should be reachable from a hub). Healthy
   means zero of each.
6. **Print the digest** — shipped / in flight / bugs / backlog / health — and
   end with **the single most leveraged next action**, chosen from what the
   sweep showed, with a one-line justification.

## Rules

- Read-only: this skill writes no files and changes no statuses; if the sweep
  finds inconsistencies (a done plan still under Active), report them as the
  next action instead of fixing silently.
- Every number in the digest traces to a query or git — no estimates.
