---
type: bug
description: 'Example fixed bug: the pause button stayed enabled after a session completed, found by an agent and closed with a Fix write-up.'
stage: done
created: 2026-07-27
completed: 2026-07-29
reported_by: agent
affected_versions:
- 0.1.0
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Bug: Pause button stays enabled after a session completes

*Example document — this shows a closed bug ticket: `stage: done` with its
`completed` date and a `## Fix` write-up, listed under `## Fixed` in the bugs
archive. The setup skill deletes `*.example.md` files after onboarding.*

## Symptom

Found 2026-07-27 by an agent writing reducer tests on 0.1.0. When a focus
session completes, the pause button in the main window stays enabled; pressing
it moves the finished session back to `paused`.

## Expected

A completed session is final and stays in history, as [Focus
sessions](../features/focus-sessions.example) describes; the "Pause and resume"
requirement in the [Timer spec](../spec/timer.example) is about a running
session.

## Reproduction

1. Start a one-minute session and let it complete.
2. Press pause in the main window.
3. The countdown shows `00:00` with a paused badge, and the history entry
   disappears.

## Root cause

The view enabled the button from `session !== null` instead of from the
session's state, and the reducer accepted `pause` in any state.

## Fix

- **Change:** the reducer ignores `pause` unless the session is running, and the
  view reads the button state from the store.
- **Guard:** `src/store/reducers.test.ts` — "pause after completion is ignored",
  which fails without the fix.
- **Shipped:** listed under `## Fixed` in [0.2.0](../releases/0.2.0.example).

## Related

- [Focus sessions plan](../plans/20260701-focus-sessions.example), which built
  the reducer.

*Convention notes: `reported_by: agent` because no one reported it — the agent
noticed it while writing tests. A ticket closed without a fix gets
`stage: cancelled`, a `## Fix` section saying why, and a link under
`## Cancelled` in the bugs archive. Either way the ticket stays in the graph.*
