---
type: learning
description: Shortcut registration fails without an error.
created: 2026-08-23
kind: trap
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Shortcut registration fails without an error

Registering a global shortcut that another app holds returns false instead of
throwing. Check the return value and tell the user which shortcut is taken.
