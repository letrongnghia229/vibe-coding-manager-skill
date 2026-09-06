# Current Phase

Keep this file short. It is the active workflow-state index, not a PRD, implementation log, or checkpoint history.

Project: <project name>
Current Phase: <phase number/name>
Previous Stable Checkpoint: <tag/commit/phase>

## Round Ledger

| Round | Name | Status |
|---|---|---|
| <N.1> | <name> | <PLANNED/FROZEN/READY_FOR_PLAN/PLAN_PASS/IMPLEMENTED/ROUND_ACCEPTED> |
| <N.2> | <name> | <status> |

## Current Round

Round: <N.x>
State: <workflow state>
Model: <Luna/Terra/Sol + effort, when selected>

Next Action:
<exactly one concrete next action>

## Working Tree / Checkpoint State

- Current Phase work: <checkpointed / uncommitted / mixed>
- Stable remote may be older than the current local working tree: <yes/no/unknown>

## Critical Guardrails

- Do not revert/discard accepted uncommitted work.
- Do not commit/tag/push before the project's acceptance/checkpoint gate unless explicitly authorized.
- Do not start the next Phase automatically.

## Maintenance Rule

Update this file only at meaningful workflow transitions. Keep detailed scope in PRD/roadmap/UI/spec docs and stable history in `CHECKPOINTS.md`.
