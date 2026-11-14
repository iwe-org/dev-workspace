---
type: plan
description: 'Example deferred plan: an accessibility pass over the countdown announcements and the tray icon.'
stage: deferred
created: 2026-09-22
tags:
- milestone-polish
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Accessibility pass

Deferred on 2026-09-25: nobody on the project can test with a screen reader
until the test machine is set up, and a fix that cannot be checked is not worth
shipping.

*Example document — this shows a deferred plan: `stage: deferred`, a first line
saying why and what it waits for, and its link under `## Deferred` in the plans
hub. The setup skill deletes `*-example.md` files after onboarding.*

## Context

An accessibility review turned up two problems, both deferred with this plan:
the [screen reader announces every
second](../bugs/screen-reader-reads-every-second-example.md) of the countdown,
and the [tray icon is invisible in high
contrast](../bugs/high-contrast-tray-icon-invisible-example.md).

## Approach

Announce the countdown once a minute and on request instead of on every tick,
and draw the tray icon as a template image the system tints for the current
contrast mode. Rejected: switching the announcements off, which leaves a
screen-reader user with no countdown at all.

## Implementation Steps

### Task 1: Countdown announcements

**Files:** Modify: `src/ui/timer-view.tsx`; Create: `src/ui/announce.test.tsx`

- [ ] Announce at each whole minute and at completion
- [ ] A shortcut that reads the remaining time on request

### Task 2: Tray icon

**Files:** Modify: `src/ui/tray.ts`, `src/ui/tray.test.ts`

- [ ] Build the icon as a template image
- [ ] Test it in light, dark and high-contrast modes

## Spec changes

[Timer](../spec/timer-example.md) — a new "Announcements" requirement: how often
the countdown is announced.

## Verification

- `npm test -- src/ui` — green.
- Manual: a full session with the screen reader on, and the tray icon checked in
  every contrast mode.

## Out of scope

- Keyboard access to every window, which has its own [backlog
  task](../backlog/keyboard-only-flow-example.md).

## Key references

Verified anchor points (line numbers as of 2026-09-22):

- `src/ui/timer-view.tsx:40` — the countdown element announced on every tick
- `src/ui/tray.ts:18` — `buildIcon()`, the fixed-colour icon
