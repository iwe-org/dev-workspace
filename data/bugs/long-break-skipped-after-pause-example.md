---
type: bug
description: The long break is skipped after a paused session.
created: 2026-09-14
reported_by: agent
affected_versions:
- 0.3.1
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The long break is skipped after a paused session

## Symptom

2026-09-14: Found during the long-break review on 0.3.1: a session that was
paused once does not count toward the long break.

## Expected

Paused and resumed sessions count like any other.

## Reproduction

1. Set a long break every 2 sessions.
2. Pause and resume the first session.
3. Complete both: a short break starts.

## Leads

1. Not investigated yet.
