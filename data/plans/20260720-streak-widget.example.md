---
created: 2026-07-20
---

# Streak widget

*Example document — this shows an active plan (no `status` frontmatter) with a
`## Depends on` section. The setup skill deletes `*.example.md` files after
onboarding.*

## Context

The [streak widget feature](../features/streak-widget.example) is proposed: an
always-on-top widget showing the daily focus streak. Streaks are derived from
the session log at read time per the
[state model](../architecture/state-model.example) — no new stored state.

## Approach

A pure `computeStreak(log, timezone)` function over the session log, rendered in
a small frameless window. Day boundaries come from local midnight at session
completion time so timezone changes never double-count.

## Implementation Steps

### Task 1: Streak derivation

**Files:** Create: `src/store/streak.ts`, `src/store/streak.test.ts`

- [ ] `computeStreak` over the session log
- [ ] Timezone-change and DST test cases

### Task 2: Widget window

**Files:** Create: `src/ui/widgets/streak.tsx`; Modify: `src/main.ts`

- [ ] Frameless always-on-top window, click opens history
- [ ] No notifications, no negative framing (concept constraint)

## Spec changes

`spec/streaks` — to be created at ship time: day-boundary and derivation
requirements from Task 1.

## Depends on

Requires the session log populated by
[Focus sessions](20260701-focus-sessions.example) — streaks derive from
completed sessions.

## Verification

- Unit tests cover streak derivation across timezone changes.
- Manual: complete a session, widget shows day 1; simulate a skipped day, streak
  restarts at zero without any alert.

## Out of scope

- Weekly/monthly aggregates in the history view.

*Convention notes: an active plan simply has no `status` — it appears under
`## Active` in the plans hub. `## Depends on` uses inline links (soft
references), so the dependency is queryable via `iwe find --references` without
making this plan a child of the other. A spec that doesn't exist yet is named in
back-ticks, not linked — links must never dangle.*
