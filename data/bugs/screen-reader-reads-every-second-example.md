---
type: bug
description: The screen reader announces every second.
stage: deferred
created: 2026-08-15
reported_by: user
affected_versions:
- 0.2.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The screen reader announces every second

## Symptom

Reported 2026-08-15 on 0.2.0. With a screen reader on, the countdown is
announced every second, which drowns out everything else.

## Expected

The countdown is announced at the minute and on request.

## Reproduction

1. Turn on the screen reader.
2. Start a session.

## Leads

1. Not investigated yet.

## Related

- [Accessibility pass](../plans/20260922-accessibility-pass-example.md), the
  deferred plan that fixes it.
