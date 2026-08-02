---
type: codebase
description: 'Example codebase map: the session lifecycle flow, from start through completion.'
source: src
commit: 3f1a9c2
stale_after: 2026-11-01
sources:
- id: code
  resource: src
  title: the code this map describes, read at commit 3f1a9c2
  author: human:author
  last_modified: 2026-07-25
verified:
  by: human:author
  at: 2026-07-25T00:00:00Z
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Flow: session lifecycle

*Example document — this shows a flow doc: a cross-cutting runtime trace that no
single component doc can give. The setup skill deletes `*.example.md` files
after onboarding.*

What happens between "start a focus session" and a row in the history log.

## Trace

1. UI start action → `createEngine(config).start()` in the
   [timer engine](timer.example) — `src/ui/timer-view.tsx:33`
2. Engine ticks on the monotonic clock; each transition fires `onTransition` —
   `src/timer/engine.ts:63`
3. The [session store](store.example) reduces transitions into state; the UI
   re-renders from subscriptions — `src/store/reducers.ts:21`
4. On `completed` (or `abandoned` past the one-minute floor), the store appends
   to the JSON-lines log — `src/store/sessions.ts:15`
5. Derived views (history, the [streak](store/streak.example)) recompute lazily
   on next read — `src/store/streak.ts:8`

Behavior contract for steps 2 and 4 lives in the
[Timer spec](../spec/timer.example).

## Failure modes

- System sleep mid-session → wake reconciliation (step 2); currently buggy, see
  [timer drift](../bugs/timer-drift-after-sleep.example).
- Crash between steps 4 and 5 → log replay on startup recovers silently.

*Convention notes: flow docs earn their keep by crossing component boundaries —
if a trace stays inside one component, it belongs in that component's doc
instead. `source` names the broadest path the trace touches.*
