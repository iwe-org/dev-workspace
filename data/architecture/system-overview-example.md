---
type: architecture
description: 'Example architecture overview: the modules under src/ and how they talk.'
sources:
- id: src
  resource: src
  title: the source tree the diagram maps
  author: human:author
  last_modified: 2026-10-02
generated:
  by: human:author
  at: 2026-10-15T00:00:00Z
---

# System overview

*Example document — this shows the overview note an architecture hub starts
with: one diagram of the modules, and a link to the note that goes deeper. The
setup skill deletes `*-example.md` files after onboarding.*

``` mermaid
flowchart TD
  Main["main.ts<br/>starts the app, wires the modules"]
  Main --> UI["ui/<br/>tray, timer view, history, widgets"]
  Main --> Shortcuts["shortcuts/<br/>global shortcuts"]
  Main --> Settings[("settings/<br/>settings file and migrations")]
  UI -- "dispatch action" --> Store["store/<br/>single store, pure reducers"]
  Shortcuts -- "dispatch action" --> Store
  Store -- "start / pause / resume" --> Timer["timer/<br/>engine and break schedule"]
  Timer -- "tick / session ended" --> Store
  Timer -- "session ended" --> Audio["audio/<br/>completion chime"]
  Store -- "append" --> Log[("Session log<br/>sessions.jsonl")]
  Store -- "subscribe" --> UI
  Settings --> Timer
  Settings --> Audio
```

The modules under `src/` and how they talk. Every change goes through the store;
how it keeps and derives state is the [state model](state-model-example.md).

*Convention notes: `sources` names the code the note was derived from, which is
what the verify skill checks it against. One diagram per note; a module that
needs its own gets its own note.*
