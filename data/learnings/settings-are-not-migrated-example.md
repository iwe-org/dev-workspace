---
type: learning
description: Settings are not migrated on update.
created: 2026-08-31
kind: correction
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Settings are not migrated on update

It was assumed that renamed settings keys carried over on update. They do not:
the app reads only the names it knows. Every rename needs a migration step,
which the [settings file
migration](../plans/20260830-settings-file-migration-example.md) plan added.
