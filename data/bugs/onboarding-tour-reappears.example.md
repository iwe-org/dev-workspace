---
type: bug
description: The onboarding tour shows again after every update.
created: 2026-10-12
reported_by: user
affected_versions:
- unreleased
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The onboarding tour shows again after every update

## Symptom

Reported 2026-10-12 on unreleased. On a build from main, the onboarding tour
opens again after each update even when it was finished.

## Expected

The tour shows once, per [Onboarding tour](../features/onboarding-tour.example).

## Reproduction

1. Finish the tour.
2. Install a newer build.
3. The tour opens again.

## Leads

1. Not investigated yet.
