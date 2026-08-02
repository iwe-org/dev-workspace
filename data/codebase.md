# 🧭 Codebase

*The map of the code as it actually is — generated and refreshed by the map
skill (ships in the [code-map template](https://github.com/iwe-org/code-map);
copy its `.claude/skills/` in to use it here), never written from memory. The
map mirrors the code's containment tree: one doc per component (crate, package,
module) at a canonical key matching its source path, children linked from their
parent's `## Contains` — so `iwe tree -k data/codebase` renders the component
tree. Every doc carries `source` (the code it describes), `commit` (the git
revision it was read at), and `verified` (the date); code newer than `commit`
means the doc is suspect — refresh it. Division of truth: spec/ is what must be,
architecture/ is why it's shaped this way, this hub is what is.*

## Getting around

*Filled by the map skill: how to build, run, and test; the entry points; a
directory → component table.*

✏️

## Components

[Timer engine](codebase/timer.example)

[Session store](codebase/store.example)

## Flows

[Flow: session lifecycle](codebase/flow-session-lifecycle.example)

## Interfaces

*External surfaces — HTTP APIs, CLI commands, storage formats, IPC contracts —
one doc each, keyed `api-<name>`.*
