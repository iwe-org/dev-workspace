---
type: bug
description: The countdown ends one second early.
stage: cancelled
status: deprecated
created: 2026-09-18
reported_by: agent
affected_versions:
- 0.3.1
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The countdown ends one second early

## Symptom

2026-09-18: Found in a flaky test run on 0.3.1: a session ended at 00:01 instead
of 00:00.

## Expected

The countdown shows 00:00 when the completion signal fires.

## Reproduction

1. Not reproduced outside the one test run.

## Fix

- **Change:** none — not reproducible. The one failure came from a test that
  slept for real on a loaded CI machine; 200 local runs never showed it.
- **Guard:** none for the product. The test itself now runs on the fake clock,
  the rule in [timer tests run on a fake
  clock](../learnings/tests-use-a-fake-clock-example.md).
- **Shipped:** nothing to ship; closed without a release entry.
