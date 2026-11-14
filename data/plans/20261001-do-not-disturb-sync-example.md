---
type: plan
description: 'Example approved plan: do-not-disturb sync, signed off and waiting to start.'
stage: approved
created: 2026-10-01
tags:
- milestone-polish
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Do-not-disturb sync

*Example document — this shows an approved plan: reviewed and ready to build,
with no box ticked yet, listed under `## Active` in the plans hub. The setup
skill deletes `*-example.md` files after onboarding.*

## Context

Builds the [Do-not-disturb sync](../features/do-not-disturb-sync-example.md)
feature for the [Polish](20260901-polish-milestone-example.md) milestone. A
spike before this plan found that the system [asks for permission on first
use](../learnings/dnd-needs-permission-first-example.md), which shapes Task 3.

## Approach

A small `dnd` module wraps the system API and is driven by the session-started
and session-ended transitions the store already emits. It remembers whether do
not disturb was on before the session and leaves it as it found it. Rejected:
toggling from the timer view, since the tray and the global shortcuts start
sessions too.

## Implementation Steps

### Task 1: System wrapper

**Files:** Create: `src/dnd/system.ts`, `src/dnd/system.test.ts`

- [ ] `turnOn`, `turnOff` and `isOn` over the system API
- [ ] Remember the state found at session start

### Task 2: Session wiring

**Files:** Modify: `src/main.ts`, `src/timer/engine.ts`

- [ ] On at session start, off at session end, completed or abandoned
- [ ] Off on wake when the session ended during sleep

### Task 3: Setting and permission

**Files:** Modify: `src/ui/settings.tsx`

- [ ] A setting, off by default
- [ ] Ask for the permission when the setting is turned on, never during a
  session

## Spec changes

`spec/do-not-disturb` — to be created at ship time: on for the session, off
after sleep, and the user's own setting wins.

## Verification

- `npm test -- src/dnd` — green against a fake system API.
- Manual: a session with the setting on, one that ends during sleep, and one
  started with do not disturb already on.

## Out of scope

- Do not disturb during breaks: a break is when messages are welcome.
- Linux, where the tray itself is a deferred [backlog
  task](../backlog/linux-tray-support-example.md).

## Key references

Verified anchor points (line numbers as of 2026-10-01):

- `src/main.ts:31` — window and tray bootstrap, where the module is wired
- `src/timer/engine.ts:91` — the wake-event handler

*Convention notes: `stage: approved` is optional — a plan with no stage is just
as open. Use it when someone signs a plan off before work starts. The spec it
will create is named in back-ticks until the ship skill writes it.*
