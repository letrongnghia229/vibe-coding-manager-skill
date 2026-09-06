# Repository Structure Policy

## Table of contents

1. Purpose and load conditions
2. Existing structure first
3. Keep top-level shallow and obvious
4. One concern, one obvious home
5. Organize by product architecture
6. Keep runtime/generated data outside the repository
7. Prefer few layers
8. Make tests discoverable
9. Keep documentation canonical
10. `AGENTS.md` is a map, not the manual
11. Nontechnical-user decision rule
12. Restructure gate
13. Token-efficiency implications

## Purpose

Guide repository organization for vibe-coded projects without forcing enterprise architecture onto small apps. Optimize for stability, discoverability, easy review, and low Codex context cost.

Load this reference only when:
- starting a new project;
- adding a major subsystem;
- the user asks how to organize the repo;
- Codex proposes a significant restructure;
- the repo has become hard to navigate.

Do not load it for routine feature/fix Rounds.

## 1. Existing structure first

If the repository is understandable and working, preserve it. Do not reorganize code merely to match a fashionable template or make the tree look more “professional.”

A structural refactor needs a concrete problem and measurable benefit, such as:
- duplicate ownership/source of truth;
- circular or tangled dependencies;
- files consistently impossible to locate;
- tests cannot be mapped to subsystems;
- a major new subsystem has no obvious home.

## 2. Keep top-level shallow and obvious

Prefer a small number of descriptive roots, for example:

```text
AGENTS.md
src/                 # or backend/ + frontend/ if that matches the app
tests/
docs/
```

Use the stack/project's existing conventions. Do not add folders that have no current contents/need.

## 3. One concern, one obvious home

Each important concern should have one primary location. Avoid two parallel modules that both appear canonical.

Examples:
- backend/business logic -> backend/src or existing backend package;
- UI -> frontend/src or existing UI package;
- migrations -> existing migration area;
- product/architecture specs -> canonical docs area;
- tests -> predictable location next to or mirroring the subsystem.

## 4. Organize by product architecture, not project-management history

Do not create code folders such as:

```text
phase7/
phase8/
round8_3/
fix_round_2/
```

Phase/Round history belongs in roadmap/checkpoint/docs. Production code should reflect stable product/subsystem concepts.

## 5. Keep runtime/generated data outside the repository

Do not store real runtime databases, caches, downloaded models, generated media, temporary exports, secrets, or user working data in the source repository unless the project explicitly defines a safe fixture/test-data exception.

Keep generated build outputs ignored unless the project intentionally tracks them.

## 6. Prefer few layers

Do not automatically create chains such as:

```text
controller -> manager -> service -> repository -> adapter -> provider -> factory
```

Use another layer only when it solves a real boundary/problem. Fewer meaningful layers usually mean:
- fewer files Codex must inspect;
- lower context/token cost;
- simpler debugging;
- fewer hidden data/control paths.

Stable and boring is preferable to abstract and clever.

## 7. Make tests discoverable

A developer/Codex inspecting a subsystem should quickly know where its tests live and how to run the focused set.

Prefer:
- predictable naming;
- test layout matching subsystem boundaries;
- focused commands documented in existing project instructions when stable.

Do not require a repository-wide scan to find the relevant regression tests.

## 8. Keep documentation canonical

Prefer one maintained source for each project truth. Avoid copies such as:

```text
PRD_v2.md
PRD_final.md
PRD_latest.md
PRD_new.md
```

Use history/version control for old states. Link to canonical docs from `AGENTS.md` instead of copying long requirements there.

## 9. `AGENTS.md` is a map, not the manual

Repository-root `AGENTS.md` should be short and durable. Include only:
- repository map;
- doc routing;
- critical stable safety constraints;
- Git/workflow rules;
- concise reporting/context-efficiency rules.

Put detailed architecture, product requirements, roadmap, UI specs, and checkpoint history in their canonical docs and load them only when relevant.

## 10. Nontechnical-user decision rule

Do not ask a nontechnical user to choose between internal folder/layer patterns when there is an obvious engineering default. ChatGPT/Codex should inspect the current repo and recommend the simplest consistent location.

Escalate to the user when a repository-structure decision materially changes:
- product behavior or workflow;
- data safety/recovery;
- cost/deployment constraints;
- long-term maintainability or a major irreversible boundary;
- an already-approved architecture/spec.

Explain those tradeoffs in plain language.

## 11. Restructure gate

Before approving a significant restructure, require a short case:
1. concrete current problem;
2. smallest structural change that solves it;
3. files/subsystems moved;
4. migration/import/build/test risks;
5. rollback/reversibility;
6. token/context impact;
7. proof that a local refactor cannot solve the problem more safely.

If that case is weak, preserve the current structure.

## 12. Token-efficiency implications

Repository structure affects Codex usage. Prefer structures that let Codex:
- find the right code with targeted search;
- inspect a small number of files for one feature;
- run focused tests;
- avoid duplicate docs/authority;
- avoid unnecessary abstraction traversal.

Do not restructure solely to save tokens; correctness and maintainability remain primary.
