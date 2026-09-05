# GitHub Usage: Stable Remote vs Current Working Tree

## Core rule

GitHub usually represents the latest pushed/stable state, while Codex may be working on newer uncommitted local changes.

Never assume:

`GitHub HEAD == current local Round state`

unless the user explicitly says the Round/Phase was committed and pushed.

## Recommended roles

### GitHub

Use for:
- stable code;
- stable project docs;
- historical checkpoints/tags;
- architecture inspection at a known ref;
- independent remote verification after checkpoint.

### Codex/local repository

Use for:
- current uncommitted implementation;
- local working-tree diff;
- runtime logs;
- tests/builds;
- current Phase state.

### ChatGPT Web

Use for:
- planning and critique;
- reviewing Codex plans/reports;
- reading stable GitHub context when connected;
- deciding acceptance gates;
- comparing current evidence against stable docs/checkpoints.

## During a Round

Do not push temporary work merely so ChatGPT can read it unless the project deliberately uses a branch-based review workflow.

Instead, ask Codex for a review package when code-level context is needed:
- `git diff --stat`;
- changed files;
- relevant diff excerpts;
- contract/data changes;
- tests added/changed;
- known risks.

## Final regression

The authoritative comparison is:

`previous stable checkpoint/tag -> current local working tree`

A GitHub stable tag alone is not enough to inspect uncommitted Phase work.

## After checkpoint

Verify:
- remote branch contains the checkpoint commit;
- remote tag exists;
- remote tag points to the intended commit;
- checkpoint docs are updated;
- local working tree is clean.

Only then treat GitHub as the new stable source of truth.
