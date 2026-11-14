---
type: plan
description: 'Example active plan: building the streak widget, one task in.'
created: 2026-07-20
tags:
- milestone-v1
generated:
  by: human:author
  at: 2026-07-20T00:00:00Z
---

# Streak widget

*Example document — this shows an active plan (no `stage` frontmatter) with a
`## Depends on` section. The setup skill deletes `*-example.md` files after
onboarding.*

## Context

The [streak widget feature](../features/streak-widget-example.md) is accepted:
an always-on-top widget showing the daily focus streak. Streaks are derived from
the session log at read time per the [state
model](../architecture/state-model-example.md) — no new stored state.

## Approach

A pure `computeStreak(log, timezone)` function over the session log, rendered in
a small frameless window. Day boundaries come from local midnight at session
completion time so timezone changes never double-count.

## Implementation Steps

### Task 1: Streak derivation

**Files:** Create: `src/store/streak.ts`, `src/store/streak.test.ts`

- [x] `computeStreak` over the session log
- [x] Timezone-change and DST test cases

### Task 2: Widget window

**Files:** Create: `src/ui/widgets/streak.tsx`; Modify: `src/main.ts`

- [ ] Frameless always-on-top window, click opens history
- [ ] No notifications, no negative framing (concept constraint)

## Spec changes

`spec/streaks` — to be created at ship time: day-boundary and derivation
requirements from Task 1.

## Depends on

Requires the session log populated by [Focus
sessions](20260701-focus-sessions-example.md) — streaks derive from completed
sessions.

## Verification

- Unit tests cover streak derivation across timezone changes.
- Manual: complete a session, widget shows day 1; simulate a skipped day, streak
  restarts at zero without any alert.

## Out of scope

- Weekly/monthly aggregates in the history view.

## Key references

Verified anchor points (line numbers as of 2026-07-20):

- `src/store/sessions.ts:15` — `appendSession()`, where the local day is stored
  with each entry
- `src/main.ts:31` — window and tray bootstrap, where the widget window is
  created

*Convention notes: an active plan simply has no `stage` — it appears under
`## Active` in the plans hub. `## Depends on` uses inline links (soft
references), so the dependency is queryable via `iwe find --references` without
making this plan a child of the other. The ticked boxes under Task 1 say where
the work stands; they do not change the stage. A spec that doesn't exist yet is
named in back-ticks, not linked — links must never dangle.*
