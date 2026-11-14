---
type: feature
description: 'Example deprecated feature: a separate tiny window showing only the countdown, replaced by the resizable main window.'
stage: deprecated
status: deprecated
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Compact mode

*Example document — this shows a deprecated feature: `stage: deprecated` with
OKF `status: deprecated`, the body saying what replaced it and when it goes. The
setup skill deletes `*-example.md` files after onboarding.*

## Purpose

A separate, tiny, always-on-top window showing only the countdown, for users who
want the timer in view without the main window.

## Behaviour

Opened from the tray menu; clicking it brings the main window back. Deprecated
in [0.4.0](../releases/example-0.4.0.md): the main window now resizes down to
the same size, which makes a second window redundant. The menu item stays for
one more release, marked as deprecated, and is then removed.

## Edge cases

- With both windows open, both show the same countdown, since both render the
  store.
- The daily goal ring is not drawn in the compact window.

## Open questions

- Remove it in the next release, or wait for a complaint?
  - Remove it; the resizable window covers every use that was reported.
