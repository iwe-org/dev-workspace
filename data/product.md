---
type: tracker
description: What the product is, who it is for, and the decisions every plan and spec derives from.
stage: living
generated:
  by: human:author
  at: 2026-08-01T00:00:00Z
---

# Product

*The foundation document. Every plan, spec, and release decision derives from
what's written here — fill it in before anything else (the setup skill walks you
through it, drafting most sections from your codebase). Replace each ✏️ block;
delete the instruction lines when done. Log every material change in the
Changelog section at the bottom.*

## What is it

*One-liner first (the sentence you'd say at a party), then a short paragraph:
what the product does, for whom, and what changes for them when it works.*

✏️

## Users

*Who actually uses it — role, context, the moment they reach for it. If there
are distinct user groups, name each one and what a win looks like for them.
Honest notes on who it is NOT for save entire feature debates later.*

✏️

## Platforms

*Where it runs and how it's distributed: OS/browser targets, minimum versions,
app stores or package registries, update mechanism. Note the platforms you have
deliberately decided not to support.*

✏️

## Stack

*Languages, frameworks, key dependencies, and the repository layout in a few
lines: where the entry points are, how the code is organized, how to build and
run the tests. This is what a fresh agent session reads first to orient in the
codebase.*

✏️

## Constraints

*The rules that bound every plan: performance budgets, offline requirements,
compatibility promises, licensing limits, security or privacy obligations. A
constraint written here is a constraint no plan has to rediscover.*

✏️

## Authoring rules

*Optional but powerful: project-specific rules the agent must follow when
writing each document type, checked by the plan and ship skills before they
write. Group by target, e.g.:*

- *specs — "every requirement touching file paths must state cross-platform
  behavior"*
- *plans — "any task touching the storage layer must include a migration step"*
- *code anchors — "verify line numbers against the current checkout, never cite
  from memory"*

✏️

## Changelog

- 2026-08-01 — document created.
