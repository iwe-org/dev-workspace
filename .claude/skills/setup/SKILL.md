---
name: setup
description: Brownfield-first workspace onboarding — scan the codebase and draft data/product.md plus starting architecture and spec docs from what the code shows, interview the developer only for what the code can't answer, mark the onboarding tasks done, and remove the example documents and everything they left behind. Use when the user says "run the setup", "set up the workspace", or when the "What is it" section of data/product.md is still an unfilled ✏️.
---

# Workspace setup

Turn the blank workspace into this project's memory. The codebase is the
primary source — read it first and draft from evidence; ask the developer only
what the code can't answer. Every future session reads what you write here.

## Steps

1. **Assess.** Read `data/product.md` and list which ✏️ blocks are unfilled. If
   `## What is it` is filled, setup has already run: ask what to revise, or
   offer to fill the sections that were skipped, instead of onboarding again.
2. **Locate the codebase, and settle git.** Ask where the project's code lives
   (often the parent or a sibling directory of this workspace, or this workspace
   sits inside the repository itself). Then check whether the workspace is
   already inside a git repository (`git rev-parse --is-inside-work-tree`). If
   it is not, ask once whether it should be one, and run `git init` only on a
   yes. A no is final: the skills validate and skip the commit. If the project
   is greenfield — no code yet — skip to step 4 and run the interview alone.
3. **Scan.** Read the repository's README, package manifests, build
   configuration, entry points, directory layout, and test setup. Draft from
   what you find:
   - `data/product.md` — What is it, Platforms, Stack from direct evidence;
     leave ✏️ plus a concrete question under any section the code can't
     answer (Users, Constraints usually need the developer).
   - One starting `data/architecture/<slug>.md` describing the module layout
     and any design decisions visible in the code (state management, storage,
     process boundaries). Say "unknown" where you'd be guessing.
   - Propose (don't yet write) spec docs for the 2–3 most load-bearing
     behaviors you can identify.
4. **Interview.** Ask in batches, conversationally:
   - *Product*: the one-liner; who uses it and for what; who it's not for.
   - *Reality*: current stage, what's shipped vs. aspirational, the next thing
     they intend to build.
   - *Constraints*: performance budgets, compatibility promises, licensing,
     privacy — anything every plan must respect.
   - *Rules*: recurring instructions they find themselves repeating to agents
     or contributors — these become the Authoring rules section. It is
     optional: if there are none, replace its ✏️ with "None yet."
5. **Write.** Fill the `data/product.md` sections, deleting the italic
   instruction lines as sections fill, and replace the bullet under
   `## Changelog` with `- <today> — product doc filled in by setup.` Write the
   confirmed architecture doc(s) and any spec stubs the developer approved,
   linking each from `data/architecture.md` / `data/spec.md`.
6. **Close the onboarding tasks.** The four onboarding tasks in
   `data/backlog/` carry the template's date; give them today's:
   `iwe update -k data/backlog/fill-product-doc -k data/backlog/capture-current-architecture -k data/backlog/import-existing-backlog -k data/backlog/plan-first-feature --set created=<today>`.
   Then mark the finished ones — `fill-product-doc` once no ✏️ is left in
   `data/product.md`, and `capture-current-architecture` if step 3 ran:
   `iwe update -k data/backlog/<slug> --set stage=done --set completed=<today>`,
   and move their links in `data/backlog.md` to `## Done`.
7. **Remove the examples.** Delete every `*-example.md` and
   `releases/example-*.md` under `data/` with `iwe delete`, which also removes
   their inclusion links from the hubs:

   ```sh
   for f in data/*/*-example.md data/releases/example-*.md; do iwe delete "${f%.md}"; done
   ```

   Then remove what the examples left behind. Each of these is a separate edit;
   do all of them:
   - `data/log.md` — replace every dated group with a single one,
     `## <today>`, holding one bullet:
     `- **Initialization**: Workspace set up — [product](product.md) filled in and the example documents removed.`
     The file needs at least one dated group to stay valid.
   - `data/plans-archive.md` — delete the `### YYYY-MM` headings under
     `## Done`; they are the months the example plans shipped in.
   - `data/releases/metrics.csv` — cut it down to its header line; its rows
     describe the example releases.
   - `AGENTS.md` — delete the **Example docs** paragraph under Conventions.
   - `SCHEMA.md` — delete the paragraph that begins "The examples below
     describe one fictional product".
   - Anywhere else: `iwe delete` turns an inline link to a deleted document
     into plain text, so a document you wrote during setup that cited an example
     now has a sentence without its link; reword or remove it. No link to an
     example may be left: `grep -rn -e '-example' -e 'example-' data index.md`
     must print nothing.

   The hub charts draw empty axes until real documents arrive, and
   `data/releases/unreleased.md` stays, empty, as the page the first shipped
   work is linked from.
8. **Stamp and validate.** Every document this run changed gets a `generated`
   stamp (`AGENTS.md`, step 5) — the hubs and the onboarding tasks included,
   which still carry the template's. Then `iwe normalize`, and
   `iwe schema validate` — both must pass clean. If the workspace is a git repository, commit with a message like
   `setup: product doc filled, architecture captured, examples removed`.

## Rules

- Never invent — every drafted statement must trace to the code or the
  developer's answer; "unknown" beats plausible fiction.
- Never leave a ✏️ block half-filled — ask follow-ups until each section is
  concrete. `## What is it` has to be filled for setup to finish. The developer
  may say "skip for now" to any other section: leave its ✏️ so the gap stays
  visible, and leave `fill-product-doc` planned. Later sessions mention the gap
  once and do not run setup again.
- Propose before bulk-writing: spec stubs and extra architecture docs get a
  one-line pitch each and the developer's yes before they exist.
- Don't delete the examples until real documents exist for the developer to
  learn the conventions from (product.md filled is the minimum).
