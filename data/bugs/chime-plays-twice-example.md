---
type: bug
description: 'Example recurring bug: the completion chime plays twice, fixed in 0.2.0 and back in 0.4.0.'
created: 2026-07-30
reported_by: user
affected_versions:
- 0.1.0
- 0.4.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The completion chime plays twice

*Example document — this shows a bug that came back: the ticket was reopened
instead of filed again, so it has no `stage`, two `affected_versions`, the
`## Fix` that did not hold and a dated `## Recurrence` entry. The setup skill
deletes `*-example.md` files after onboarding.*

## Symptom

Reported 2026-07-30 on 0.1.0. Seen again on 0.4.0: when a session completes
while the history view is open, the chime plays twice.

## Expected

One chime per completed session.

## Reproduction

1. Open the history view.
2. Let a session complete.
3. Two chimes play.

## Root cause

Every window subscribed to the completion event and played the chime itself.

## Fix

- **Change:** the main window alone plays the chime, once per completed session;
  the other windows stopped subscribing to the completion event.
- **Guard:** `src/audio/chime.test.ts` — "one chime per completed session". It
  mounts the main window only, which is how the recurrence got past it.
- **Shipped:** commit `8be02f4`, listed under `## Fixed` in
  [0.2.0](../releases/example-0.2.0.md).

## Recurrence

- 2026-10-06, on 0.4.0: the fix shipped in 0.2.0 held through 0.3.1. 0.4.0 gave
  the history view its own subscription to the completion event, and it plays
  the chime itself. The ticket is open again, and the guard needs a second
  window.
