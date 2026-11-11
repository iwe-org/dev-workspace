---
type: learning
description: 'Example rule: a streak day is the local calendar day at the moment a session completes, never the UTC day.'
created: 2026-07-21
kind: rule
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# A streak day is the local day a session completes on

Count a session toward the calendar day of its completion time in the user's
local time zone at that moment, and store that day with the session rather than
deriving it again later. Deriving days from UTC timestamps, or from the current
time zone after a trip, double-counts or skips a day. The rule was settled while
planning the [streak widget](../plans/20260720-streak-widget-example.md) and
binds every feature that groups sessions by day.

*Example document — a `rule` learning records a standing instruction stated
once, so no later session has to be told again.*
