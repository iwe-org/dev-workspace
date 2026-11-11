---
type: hub
description: System design notes and the reasoning behind them, rejected alternatives included.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# 🏛️ Architecture

*System design notes — data structures, module boundaries, pipelines, and the
reasoning behind them, one topic per document in `architecture/<slug>.md`.
Frontmatter is `type: architecture`; like specs, these are durable reference.
Record a design decision here the moment it's made — the alternatives you
rejected are as valuable as the one you picked.*

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

*The modules under `src/` and how they talk. Every change goes through the
store; how it keeps and derives state is the [state
model](architecture/state-model-example.md).*

[State model](architecture/state-model-example.md)
