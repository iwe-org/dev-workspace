# Bug: Timer drifts after laptop sleep

*Example document — this shows the shape of a bug report; absent `status`
frontmatter means the bug is open. The setup skill deletes `*.example.md` files
after onboarding.*

## Symptom

After the laptop sleeps during a focus session, the timer sometimes shows more
remaining time than before sleep, or jumps straight to negative values.

## Reproduction

1. Start a 25-minute session, wait until ~10:00 remaining.
2. Close the lid for 5+ minutes, reopen.
3. Timer shows 10:04, 10:31, or `-42:17` depending on sleep length.

## Root cause

The tick handler recomputes remaining time from `Date.now()` deltas, so the wall
clock leaks into elapsed time across sleep — a direct violation of the
"Wall-clock independence" requirement in the
[Timer spec](../spec/timer.example).

## Fix

Compute elapsed time from `performance.now()` (monotonic) and reconcile once on
the wake event instead of every tick.

## Key references

Verified anchor points (line numbers as of 2026-07-25):

- `src/timer/engine.ts:42` — `tick()`, the wall-clock delta
- `src/timer/engine.ts:88` — wake-event handler, currently a no-op

*Convention notes: code anchors are `path:line — symbol`, stamped with the date
they were verified, so a reader knows how much to trust them. When the fix
ships, the bug gets `status: done` and stays listed in the bugs hub.*
