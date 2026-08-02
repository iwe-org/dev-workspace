---
name: setup
description: Brownfield-first workspace onboarding — scan the codebase and draft data/product.md plus starting architecture and spec docs from what the code shows, interview the developer only for what the code can't answer, mark the onboarding tasks done, and delete the example documents. Use when the user says "run the setup", "set up the workspace", or when product.md still contains ✏️ placeholders.
---

# Workspace setup

Turn the blank workspace into this project's memory. The codebase is the
primary source — read it first and draft from evidence; ask the developer only
what the code can't answer. Every future session reads what you write here.

## Steps

1. **Assess.** Read `data/product.md`. List which ✏️ blocks are unfilled. If
   everything is filled, ask what to revise instead of re-onboarding.
2. **Locate the codebase.** Ask where the project's code lives (often the
   parent or a sibling directory of this workspace, or this workspace may sit
   inside the repo itself). If the project is greenfield — no code yet — skip
   to step 4 and run the interview alone.
3. **Scan.** Read the repository's README, package manifests, build
   configuration, entry points, directory layout, and test setup. Draft from
   what you find:
   - `data/product.md` — What is it, Platforms, Stack from direct evidence;
     leave ✏️ plus a concrete question under any section the code can't
     answer (Users, Constraints usually need the developer).
   - One starting `data/architecture/<slug>.md` describing the module layout
     and any design decisions visible in the code (state management, storage,
     process boundaries). Say "unknown" where you'd be guessing. (The deep,
     per-module map under `data/codebase/` is out of scope for setup — file it
     as follow-up work rather than attempting it here.)
   - Propose (don't yet write) spec docs for the 2–3 most load-bearing
     behaviors you can identify.
4. **Interview.** Ask in batches, conversationally:
   - *Product*: the one-liner; who uses it and for what; who it's not for.
   - *Reality*: current stage, what's shipped vs. aspirational, the next thing
     they intend to build.
   - *Constraints*: performance budgets, compatibility promises, licensing,
     privacy — anything every plan must respect.
   - *Rules*: recurring instructions they find themselves repeating to agents
     or contributors — these become the Authoring rules section.
5. **Write.** Fill every `data/product.md` section, deleting the italic
   instruction lines as sections fill; add a dated entry to its Changelog.
   Write the confirmed architecture doc(s) and any spec stubs the developer
   approved, linking each from `data/architecture.md` / `data/spec.md`.
6. **Close the loop.** Mark the finished onboarding tasks
   (`fill-product-doc`, and `capture-current-architecture` if step 3 ran):
   `iwe update -k data/backlog/<slug> --set stage=done --set completed=<today>`,
   and move their links in `data/backlog.md` to `## Done`. Delete the example
   docs — every `*.example.md` under `data/` (`iwe delete <key>` per doc).
   `iwe delete` removes their inclusion links from hubs automatically but
   *flattens* inline links to plain text: sweep the hub files and remaining
   docs for leftover de-linked example lines and remove them.
7. **Validate & commit.** `iwe normalize`, then `iwe schema validate` — both
   must pass clean. Commit with a message like
   `setup: product doc filled, architecture captured, examples removed`.

## Rules

- Never invent — every drafted statement must trace to the code or the
  developer's answer; "unknown" beats plausible fiction.
- Never leave a ✏️ block half-filled — ask follow-ups until each section is
  concrete, but let the user say "skip for now" (leave the ✏️ so the gap stays
  visible).
- Propose before bulk-writing: spec stubs and extra architecture docs get a
  one-line pitch each and the developer's yes before they exist.
- Don't delete the examples until real documents exist for the developer to
  learn the conventions from (product.md filled is the minimum).
