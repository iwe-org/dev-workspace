---
type: learning
description: 'Example trap: the tray menu reads its labels once when it opens, so a live countdown there must refresh on the open event.'
created: 2026-07-12
kind: trap
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# The tray menu reads its labels once, when it opens

Pomodux's tray menu builds its items when the user opens it and never redraws
them while it is open, so a countdown pushed into a menu item on every tick
stays frozen at the value it had on open. The countdown in the tray icon's own
title does update live. Build the menu items from the store in the menu's open
handler, and keep per-tick updates for the icon title only. Hit while building
the tray controls in [Focus sessions](../plans/20260701-focus-sessions.example).

*Example document — this shows a learning: one fact per page, dated when it was
discovered, with its `kind`. It links to the plan it came from instead of
repeating it. The setup skill deletes `*.example.md` files after onboarding.*
