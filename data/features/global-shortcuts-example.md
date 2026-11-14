---
type: feature
description: System-wide shortcuts to start, pause and abandon a session without switching windows.
stage: implemented
tests:
- src/shortcuts/register.test.ts
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Global shortcuts

## Purpose

Start, pause and abandon a session from any application, without bringing
Pomodux to the front.

## Behaviour

Three system-wide shortcuts, each assignable in settings. They are registered
once at launch and again only when the user changes one. A shortcut dispatches
the same action as the matching button, so the two can never disagree. Shipped
in [0.3.0](../releases/example-0.3.0.md).

## Edge cases

- A shortcut another application holds cannot be registered, and [the system
  does not say
  so](../learnings/shortcut-registration-fails-silently-example.md): settings
  names the shortcut that is taken.
- Shortcuts are inactive while a settings field is recording a new one.

## Open questions

- Which defaults? The current start shortcut [clashes with code
  editors](../bugs/shortcut-conflicts-with-editor-example.md).
