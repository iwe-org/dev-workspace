---
type: feature
description: 'Example proposed feature: choosing the completion and break chimes, still awaiting a decision.'
stage: proposed
status: draft
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Custom sounds

*Example document — this shows a feature still in proposal: `stage: proposed`
with OKF `status: draft`, and an open question that has to be answered before it
can be accepted. The setup skill deletes `*-example.md` files after onboarding.*

## Purpose

Let the user pick the completion and break chimes, from a small bundled set or
from a file of their own.

## Behaviour

Two pickers in settings, each with a preview button. The chosen sounds are
loaded at launch like the default ones, for the reason [audio must be loaded
before the first chime](../learnings/audio-needs-preloading-example.md) gives.

## Edge cases

- The chosen file is missing at launch: the default chime plays, and settings
  says so.
- A long file is cut off after five seconds.

## Open questions

- Is a file picker, and the audio formats it brings along, worth it, or are
  three bundled chimes enough?
  - Undecided, which is why the feature is still proposed.

*Convention notes: `stage: proposed` means design discussion, and nothing is
planned against it. When a plan is filed, the plan skill sets `stage: accepted`
and clears `status: draft` in the same edit.*
