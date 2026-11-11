---
type: bug
description: The tray tooltip cuts long task labels.
created: 2026-10-08
reported_by: agent
affected_versions:
- 0.4.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The tray tooltip cuts long task labels

## Symptom

2026-10-08: Found while reviewing tray code on 0.4.0: task labels longer than 30
characters end mid-word in the tooltip.

## Expected

Long labels end with an ellipsis at a word boundary.

## Reproduction

1. Name a task with a 40-character label.
2. Hover over the tray icon.

## Leads

1. Not investigated yet.
