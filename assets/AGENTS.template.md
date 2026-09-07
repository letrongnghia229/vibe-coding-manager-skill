# <Project> — Codex Instructions

## Communication
- Respond entirely in Vietnamese unless explicitly asked otherwise.
- Assume the user may have no software-development background; keep explanations short, plain, and practical.
- Keep precise English technical terms when they are standard or clearer.
- Do not translate file names, paths, commands, code, APIs, identifiers, error codes, or model names.
- Do not restate approved plans or paste large diffs/logs unless needed.
- For a blocker, STOP condition, invalidated assumption, architecture conflict, migration/data-safety issue, recovery/concurrency/security/destructive risk, or failed high-risk implementation, start with `Giải thích cho người không có nền tảng kỹ thuật` before technical evidence.
- In that summary, state: the practical problem, one simple example/analogy when the mechanism is complex, worst-case consequence, whether any damage is confirmed or only possible, why work must stop/change, and whether the user has a real product/workflow decision to make.
- Do not ask the user to choose low-level implementation details when there is an obvious engineering default and no meaningful product trade-off.

## Repository map
- `<src/backend/etc.>` — <main responsibility>
- `<tests>` — <test location>
- `<docs>` — <project sources of truth>

Do not reorganize the repository without a concrete approved need.

## Sources of truth
- If present, read `docs/CURRENT_PHASE.md` for the active Phase/Round and next action.
- Read detailed project docs only when they are directly relevant to the current task.
- Never enumerate/read all PRD/roadmap/UI/checkpoint docs by default.
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
- If a small fix unexpectedly requires a major new dependency/subsystem/architecture, STOP and report the complexity escalation before implementing it.

## Git safety
- Do not commit/tag/push without explicit authorization.
- Do not start the next Round/Phase automatically.

## Token / credit efficiency
- Inspect narrowly first (`git diff`, `git status`, targeted search/files).
- High-risk work increases verification/review depth, not default documentation breadth.
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

If item 4 contains a blocker/STOP/high-risk invalidated assumption, prepend the plain-language summary required above.
