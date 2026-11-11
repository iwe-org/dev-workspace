---
type: learning
description: Timer tests run on a fake clock.
created: 2026-08-04
kind: rule
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Timer tests run on a fake clock

Every test that involves time uses the fake clock in `src/test/clock.ts`; a test
that sleeps for real is slow and flaky, as the [cancelled one-second
bug](../bugs/countdown-off-by-one-second-example.md) showed.
