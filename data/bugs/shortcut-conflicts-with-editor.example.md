---
type: bug
description: The default shortcut clashes with code editors.
created: 2026-09-06
reported_by: user
affected_versions:
- 0.3.0
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Bug: The default shortcut clashes with code editors

## Symptom

Reported 2026-09-06 on 0.3.0. The default start shortcut is also a common editor
shortcut, so pressing it in an editor starts a session.

## Expected

Defaults avoid shortcuts that popular editors already use, per [Global
shortcuts](../features/global-shortcuts.example).

## Reproduction

1. Install 0.3.0 with default settings.
2. Press the start shortcut in a code editor.

## Leads

1. Not investigated yet.
