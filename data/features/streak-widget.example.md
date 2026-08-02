---
status: proposed
---

# Streak widget

*Example document — this shows the shape of a feature doc still in proposal. The
setup skill deletes `*.example.md` files after onboarding.*

## Purpose

A small always-on-top widget showing the current daily focus streak, derived
from the session log.

## Behaviour

Displays consecutive days with at least one completed session. Clicking opens
the history view. Per [Why Pomodux](../concept/why-pomodux.example), the streak
never shames: a broken streak simply restarts at zero with no red badges or
"streak lost" notifications.

## Edge cases

- Timezone changes must not double-count or skip a day (derive days from local
  midnight at session completion time).

## Open questions

- Opt-in or on by default?

*Convention notes: `status: proposed` means design discussion; `accepted` means
ready to plan. The implementation work is sequenced in the
[streak widget plan](../plans/20260720-streak-widget.example).*
