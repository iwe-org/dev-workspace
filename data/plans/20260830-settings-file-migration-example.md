---
type: plan
description: 'Example shipped plan that fixes a bug: a versioned settings file with migrations, so a renamed key survives an update.'
stage: done
created: 2026-08-30
completed: 2026-09-02
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# Settings file migration

*Example document — a plan that grew out of a bug ticket rather than a feature,
and whose honest `## Spec changes` is "none". The setup skill deletes
`*-example.md` files after onboarding.*

## Context

0.3.0 renamed two settings keys and read only the new names, so the update put
session length and shortcuts back to their defaults: [settings reset after an
update](../bugs/settings-reset-after-update-example.md). The fix has to cover
this rename and every later one, which makes it a plan rather than a patch.

## Approach

A `schemaVersion` number in the settings file and an ordered list of migrations,
each a pure function from one version's shape to the next, run at load before
anything reads a setting. Rejected: reading both the old and the new key
wherever a setting is used, which fixes this rename and leaves the next one to
be forgotten the same way.

## Implementation Steps

### Task 1: Versioned settings file

**Files:** Modify: `src/settings/load.ts`, `src/settings/defaults.ts`

- [x] Write `schemaVersion` on every save; a file without one is version 1
- [x] Run the pending migrations at load, then save the migrated file

### Task 2: The 0.3.0 rename

**Files:** Create: `src/settings/migrate.ts`, `src/settings/migrate.test.ts`

- [x] Migration 1 → 2: `sessionMinutes` becomes `focusMinutes`, `hotkeys`
  becomes `shortcuts`
- [x] Tests: a 0.2.0 file keeps its values, a current file is left alone, a file
  from a newer version is left untouched and reported

## Spec changes

None — no behavioural change. Settings surviving an update was always the
intended behaviour; the release only broke it.

## Verification

- `npm test -- src/settings` — the migration tests are green.
- Manual: change the session length on 0.2.0, install the new build, and the
  length is kept.

## Out of scope

- The session log: its entries are never rewritten, so it needs no migration.
- Exporting or importing settings.

## Key references

Verified anchor points (line numbers as of 2026-08-30):

- `src/settings/load.ts:12` — `loadSettings()`, where the migrations run
- `src/settings/defaults.ts:4` — `defaults`, the keys 0.3.0 renamed
