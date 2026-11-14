---
name: weekly
description: Compose a weekly project digest from the workspace — what shipped, active plans and their staleness, open and recurring bugs, top backlog, new learnings, and graph health, ending with the single most leveraged next action. Read-only; prints without writing files. Use when the user asks for a "weekly digest", "status report", or a start-of-week review.
---

# Weekly digest

A read-only sweep of the graph that answers: what moved, what's stuck, what's
next. Numbers come from frontmatter, and from git when the workspace is a git
repository (`git rev-parse --is-inside-work-tree`) — never from memory.

## Steps

1. **Shipped.** Plans and bugs completed in the last 7 days:
   `iwe find --filter '{type: {$in: [plan, bug]}, stage: done, completed: {$gte: <YYYY-MM-DD, 7 days ago>}}' --project completed`.
   Releases cut:
   `iwe find --filter '{type: release, date: {$gte: <YYYY-MM-DD, 7 days ago>}}' --project version,date`.
   In a git repository, cross-check with
   `git log --since '7 days ago' --oneline`.
2. **In flight.** Active plans (no `stage`, or `approved`) under `## Active` in
   `data/plans.md`; flag any untouched for 14+ days as possibly stalled or
   silently done. Last touched is the last commit on the file
   (`git log -1 --format=%cs -- data/plans/<key>.md`) in a git repository, and
   otherwise the plan's `generated.at`, or the file's modification date when
   that is missing.
3. **Open bugs.** Tickets under `## Open` in `data/bugs.md`, split by
   `reported_by` (user-reported first), plus any with two or more
   `affected_versions` — those came back after a fix.
4. **Backlog head.** `## High` items in `data/backlog.md`, plus how long
   they've sat (`created`).
5. **Learnings.** Learnings dated this week, one line each:
   `iwe find --filter '{type: learning, created: {$gte: <YYYY-MM-DD, 7 days ago>}}' --project created`.
6. **Graph health.** `iwe schema validate` (report violations) and `iwe stats`.
   `iwe stats` prints a `## Broken Links` section and a `## Orphans` section
   only when there is something to list. Healthy means no violations, no
   `## Broken Links` section, and no `data/` document under `## Orphans`: every
   document under `data/` is reachable from a hub, the hubs from
   `data/index.md`, and `data/index.md` and `data/log.md` from the start page.
7. **Print the digest** — shipped / in flight / bugs / backlog / learnings /
   health — and end with **the single most leveraged next action**, chosen
   from what the sweep showed, with a one-line justification.

## Rules

- Read-only: this skill writes no files and changes no statuses; if the sweep
  finds inconsistencies (a done plan still under Active, a fixed bug still
  under Open), report them as the next action instead of fixing silently.
- Every number in the digest traces to a query, or to git where the workspace
  has it — no estimates.
