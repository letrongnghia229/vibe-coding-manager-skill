# <Project> — Codex Instructions

## Communication
- Respond entirely in Vietnamese unless explicitly asked otherwise.
- Assume the user may have no software-development background; keep explanations short, plain, and practical.
- Keep precise English technical terms when they are standard or clearer.
- Do not translate file names, paths, commands, code, APIs, identifiers, error codes, or model names.
- Do not restate approved plans or paste large diffs/logs unless needed.

## Repository map
- `<src/backend/etc.>` — <main responsibility>
- `<tests>` — <test location>
- `<docs>` — <project sources of truth>

Do not reorganize the repository without a concrete approved need.

## Sources of truth
Read only when relevant:
- `<project docs>`

The current local working tree may be newer than the stable remote. Do not discard uncommitted work merely to match the remote.

## Stable product/repository constraints
- <platform / data-safety / source-write constraints>
- Prefer stable, small, testable, reversible solutions.
- Do not introduce out-of-scope future architecture.

## Workflow
- One Round = one main goal.
- Use `/plan` before `/goal` for non-trivial work.
- `/plan` inspects only; `/goal` implements the approved plan from the same session.
- Stop if repository reality materially conflicts with the approved plan.
- Do not refactor unrelated stable code.

## Git safety
- Do not commit/tag/push without explicit authorization.
- Do not start the next Round/Phase automatically.

## Token / credit efficiency
- Inspect narrowly first (`git diff`, `git status`, targeted search/files).
- Reference docs by path instead of repeating them.
- Run focused tests first; broaden only at the appropriate risk/gate.
- Do not repeat verification after manual PASS when no code changed.
- Avoid unnecessary multi-agent/Best-of-N/tool expansion.

## Implementation report
Report only:
1. files changed;
2. behavior changed;
3. exact verification results;
4. deviations/blockers/risks;
5. manual QA steps.
