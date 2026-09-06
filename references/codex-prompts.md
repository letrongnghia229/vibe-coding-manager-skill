# Codex Prompt Templates — Compact by Default

## Table of contents

1. Principle
2. Normal Round `/plan`
3. Normal Round `/goal` and correction delta
4. UI Round additions
5. Diagnose-only
6. Manual-smoke discrepancy
7. Confirmed Fix Round `/plan`
8. Confirmed Fix Round `/goal`
9. Independent implementation review
10. Final Regression Review
11. Final Verification
12. Final Delta Verification
13. Checkpoint
14. Compact implementation report request

## Principle

Prompts should carry only information Codex does **not** already have from:

1. repository `AGENTS.md`;
2. relevant project docs;
3. the active Codex session;
4. the approved `/plan` in that session.

Do not repeat stable project rules or the full approved plan unless context was lost/restarted.

Ask Codex to inspect the smallest relevant surface first and keep reports concise.

For token/credit policy, see `token-efficiency.md`.

## 1. Normal Round `/plan`

```text
/plan

[ROUND NAME]

Read AGENTS.md and only the relevant project docs/files.

GOAL:
<one concise goal>

CURRENT GAP / EVIDENCE:
<only current behavior, screenshot/log/diff facts>

ACCEPTANCE:
1. ...
2. ...

PLAN ONLY.
Return: root cause/current flow, smallest safe change, files, risks, migration/backward-compatibility impact, tests.
Do not modify files. Do not commit/tag/push. Do not start the next Round/Phase.
Stop for review.
```

Add scope/non-goals only when they are not already obvious from `AGENTS.md`/project docs or when scope creep is a material risk.

## 2. Normal Round `/goal`

Generate only after `/plan` PASS and preferably in the **same Codex session**.

```text
/goal

[ROUND NAME]

Implement the approved plan from this session.

REVIEW DELTAS / GUARDRAILS:
- <only corrections/constraints added during plan review; omit if none>

VERIFICATION:
- focused tests for changed behavior;
- directly relevant regressions;
- broaden only if Round risk requires it;
- git diff --check.

If repository reality materially conflicts with the approved plan, STOP and report before improvising.
Do not commit/tag/push. Do not start the next Round/Phase.

Report concisely: files changed, behavior changed, exact PASS/FAIL verification, deviations/risks, manual QA steps.
Do not restate the plan or paste large diffs/logs.
```

### When the `/goal` session is not the `/plan` session

Provide a **compact approved-plan summary**, not the full plan transcript:

```text
APPROVED PLAN SUMMARY:
- goal: ...
- canonical change point: ...
- critical guardrails: ...
- acceptance: ...
```

### When a plan needs revision

Prefer a correction delta instead of restating the whole plan:

```text
/plan

CORRECTION DELTA ONLY

Keep the previous plan unchanged except:
1. <correction>
2. <correction>

Return only the corrected contract/flow, affected tests/risks, and confirmation that all other plan sections remain unchanged.
PLAN ONLY. Do not modify files.
```

Request a full rewritten plan only when the original plan is structurally invalid or the session lost reliable context.

## 3. UI Round additions

Add to `/plan` only when the UI Design Gate applies:

```text
UI SOURCE OF TRUTH:
Read the approved Master/page UI spec. Preserve product semantics and existing tokens/components where practical.
Do not invent a new visual direction.
Plan layout/scroll/responsive/accessibility/loading/empty/error states without unrelated backend refactors.
```

Add to `/goal`:

```text
UI GUARDRAIL:
Implement the approved Master/page spec; do not redesign it or change API/database behavior outside the approved plan.
Stop for Functional QA + Visual QA.
```

## 4. Diagnose-only

```text
DIAGNOSE ONLY

PROBLEM:
<actual behavior>

EVIDENCE:
<only relevant screenshot/log/runtime facts>

Inspect runtime/state/logs/database/code/spec as needed, narrowly first.
Return:
- CONFIRMED
- LIKELY
- UNKNOWN
- smallest next action

Do not modify code/data. Do not commit/tag/push.
```

## 5. Manual-smoke discrepancy

```text
MANUAL SMOKE INSPECTION — NO EDITS

EXPECTED: <...>
ACTUAL: <...>
EVIDENCE: <...>

Trace accepted requirement -> implementation -> user workflow.
Classify exactly:
A. INTENTIONAL SCOPE
B. DOCUMENTED DEFERRED
C. IMPLEMENTATION GAP

If C, identify the smallest Fix Round scope.
Do not create a workaround and call it satisfied.
```

## 6. Confirmed Fix Round `/plan`

```text
/plan

[FIX ROUND]

CONFIRMED GAP:
<root cause/evidence>

REQUIRED FIX:
<smallest expected correction>

PRESERVE:
<accepted behavior at risk>

PLAN ONLY.
Return: exact fix point, smallest change, files, tests, risks.
Do not modify files. Do not commit/tag/push. Stop for review.
```

## 7. Confirmed Fix Round `/goal`

```text
/goal

Implement the approved Fix Round plan from this session.

REVIEW DELTAS:
- <only new guardrails, if any>

Add a focused regression test when practical.
Run focused + direct regression tests; broaden only if the fix touches high-risk persistence/recovery/concurrency/security/destructive behavior.
Do not commit/tag/push.
Stop for manual retest of the failed acceptance step.
Report concise results; do not restate the plan.
```

## 8. Independent implementation review

Use only when risk justifies a separate reviewer.

```text
INDEPENDENT REVIEW — NO EDITS

[ROUND NAME]

Use current local worktree as authority for uncommitted work. Read AGENTS.md and relevant docs.
Review the implementation against the accepted Round behavior, especially:
- correctness/canonical flow;
- migration/data safety if relevant;
- recovery/concurrency/destructive risk if relevant;
- scope/dead complexity;
- tests and regressions.

Return:
VERDICT: PASS or REVISE
BLOCKERS: <NONE or numbered>
NON-BLOCKING: <short>
MANUAL-QA READINESS: YES/NO
TEST EVIDENCE: <exact commands/results>

Do not block for style. Do not modify files. Do not commit/tag/push.
```

For high-risk reviews, add only the specific frozen invariants that are not already in repo docs/current approved plan.

## 9. Final Regression Review

```text
/plan

PHASE [N] — FINAL REGRESSION REVIEW

PREVIOUS STABLE CHECKPOINT: <tag/commit>
CURRENT: all planned Rounds accepted; current local worktree contains uncommitted Phase work.

REVIEW ONLY.
Review stable checkpoint -> current worktree for real blockers: regressions, duplicate authority, migration/data safety, persistence/recovery, concurrency/races, destructive filesystem, security, user-data preservation, missing critical tests, complexity that threatens stability.

Return only:
A. confirmed-correct areas
B. blockers
C. non-blocking observations
D. exact final verification set
E. READY FOR FINAL VERIFICATION or MUST FIX BEFORE CHECKPOINT

Do not modify files. Do not commit/tag/push. Do not start next Phase.
```

## 10. Final Verification

Use only after final review has no blocker.

```text
/goal

PHASE [N] — FINAL VERIFICATION

Run the approved final verification set only.
Do not add features or refactor unrelated code.
Report exact PASS/FAIL evidence, git diff --check, and working-tree status.
Do not commit/tag/push yet unless final manual acceptance is already complete and checkpointing is explicitly authorized.
Stop for final manual smoke.
```

Do not repeat a full verification cycle after final manual PASS if **no code changed** since this verification.

## 11. Final Delta Verification

```text
FINAL DELTA VERIFICATION

A narrow post-review fix changed the worktree.
Verify only the delta + direct regression surface unless evidence shows broader risk.

If the fix touches migration/schema, canonical data, persistence/recovery, concurrency/job lifecycle, destructive filesystem, security, or broad architecture, STOP and require broader final review/verification.

Do not commit/tag/push.
```

## 12. Checkpoint

```text
FINAL MANUAL ACCEPTANCE: PASS

Create checkpoint: <TAG>

If no code changed since the last successful final verification, do not rerun the same expensive suite solely for duplication unless project policy requires it.
Always run/confirm the minimum pre-Git safety checks required by the project (at least git diff --check/status and any explicitly mandated build/test gate).

If required checks pass:
- update checkpoint/release docs;
- commit accepted Phase changes;
- create exact tag;
- push intended branch + tag;
- verify remote commit/tag;
- verify clean local worktree.

If anything fails, STOP before declaring checkpoint complete.
Do not start the next Phase.
```

## 13. Compact implementation report request

Use when Codex tends to over-report:

```text
Báo cáo ngắn gọn bằng tiếng Việt:
1. file đã đổi;
2. hành vi đã đổi;
3. test/build/smoke + PASS/FAIL;
4. deviation/blocker/known risk;
5. manual QA.
Không lặp lại plan. Không paste diff/log dài nếu không có lỗi cần xem.
```
