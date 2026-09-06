# Codex Prompt Templates

## Table of contents

0. Shared language and simplicity block
1. Normal Round `/plan`
2. Normal Round `/goal`
3. UI Round `/plan`
4. UI Round `/goal`
5. Diagnose-only
6. Manual Smoke Inspection
7. Confirmed Fix Round `/plan`
8. Confirmed Fix Round `/goal`
9. Final Regression Review `/plan`
10. Final Verification `/goal`
11. Final Delta Verification
12. Checkpoint


Use these as adaptable templates. Keep only the sections relevant to the current project/task.

## 0. Shared language and simplicity block

For a non-technical user, prepend or include this block in every substantial Codex prompt. Replace `<USER_LANGUAGE>` with the user's current language.

```text
LANGUAGE / USER LEVEL:
- Respond entirely in <USER_LANGUAGE>.
- The user does not have a software-development background.
- Explain important technical terms briefly in plain language.
- Focus on what a mechanism does and why it is necessary.
- Keep file names, function names, class names, API names, states, and code identifiers unchanged.

SIMPLICITY POLICY:
- Prefer the smallest stable solution that satisfies the accepted current requirement.
- Do not add future-proof infrastructure for hypothetical future needs.
- Do not introduce a scheduler, global state machine, exact-once machinery, global revision tracking, reconciliation layers, or new migrations unless the current requirement genuinely needs them.
- If the implementation becomes materially more complex than the user-visible goal suggests, STOP and explain why a simpler solution is insufficient before proceeding.
- A rare, recoverable, non-destructive imperfection is acceptable when avoiding it would require disproportionate architecture.

At the end, add a short section titled:
PLAIN-LANGUAGE SUMMARY FOR A NON-TECHNICAL USER
Explain in at most 10 lines what is proposed/done, why it is needed, the realistic remaining risk, and the user's next action.
```

When the user's language is Vietnamese, translate the block itself into Vietnamese in the generated prompt. Do not leave the operational prompt in English unless the user requested English.

## 1. Normal Round `/plan`

Include the Shared language and simplicity block when the user is non-technical.

```text
/plan

[TASK / ROUND NAME]

CONTEXT:
<current Phase/Round, stable baseline, relevant docs>

CURRENT PROBLEM:
<actual behavior / technical issue>

EXPECTED:
<desired behavior>

SCOPE:
<what may change>

NON-GOALS:
<what must not change>

ACCEPTANCE CRITERIA:
1. ...
2. ...

Read the relevant project docs and inspect the current implementation.

PLAN ONLY.

Return:
1. Root cause.
2. Relevant data/state/control flow.
3. Smallest viable solution.
4. Why any proposed new architecture/state/migration is actually necessary.
5. Worst realistic consequence of choosing the simpler solution.
6. Files/components/functions expected to change.
7. Migration/backward-compatibility implications.
8. Regression risks.
9. Tests to add/run.
10. Smallest safe implementation plan.

DO NOT modify files.
DO NOT commit/tag/push.
DO NOT start the next Round/Phase.
Stop after the plan for review.
```

## 2. Normal Round `/goal`

Include the Shared language and simplicity block when the user is non-technical.

Generate this only after the plan is approved.

```text
/goal

[TASK / ROUND NAME]

Implement the approved plan from this session.

BASELINE / CURRENT STATUS:
<...>

CORE GOAL:
<...>

SCOPE:
<...>

NON-GOALS:
<...>

APPROVED PLAN GUARDRAILS:
<merge any review corrections or extra constraints here>

REQUIRED BEHAVIOR / ACCEPTANCE:
1. ...
2. ...

REGRESSION PROTECTION:
- preserve previously accepted behavior;
- preserve project-specific data/state/safety constraints;
- do not refactor unrelated stable code;
- do not reintroduce complexity that the approved plan intentionally avoided;
- if implementation reality requires materially broader architecture, STOP and report instead of improvising.

TESTS / VERIFICATION:
- focused tests;
- relevant regression tests;
- production build when applicable;
- smoke tests when applicable;
- git diff --check.

If a requirement conflicts with the approved plan or repository reality, STOP and ask.

DO NOT commit/tag/push.
DO NOT start the next Round/Phase.

After automated verification, STOP and report:
1. root cause addressed;
2. files changed;
3. behavior changed;
4. test/build/smoke results;
5. known issues;
6. simple manual acceptance steps for the user.
```

## 3. UI Round `/plan`

Add this block to the normal `/plan` when the UI Design Gate has been approved:

```text
UI SOURCE OF TRUTH:
- Read the approved Master Design System and page-level UI spec first.
- Preserve existing product/interaction semantics unless explicitly changed.
- Do not invent a new visual style.
- If ui-ux-pro-max is installed, use it only as design intelligence consistent with the approved Master/UI spec.

Inspect:
- information hierarchy;
- layout/scroll ownership;
- responsive behavior;
- shared components/tokens;
- accessibility/focus states;
- loading/empty/error states;
- existing component/state boundaries.

The plan must explain how the UI can be implemented without unnecessary business-logic or backend refactors.
```

## 4. UI Round `/goal`

Add this block to the normal `/goal`:

```text
UI IMPLEMENTATION RULES:
- Follow the approved Master Design System and page/UI spec.
- Reuse approved shared components/tokens where practical.
- Do not regenerate or replace the Master Design System.
- Do not change API/database/business logic unless the approved plan requires it.
- Preserve keyboard/focus/accessibility behavior.
- Prevent page-level horizontal overflow unless explicitly intended.
- Add/update visual and interaction tests where practical.
- After implementation, stop for Functional QA + Visual QA.
```

## 5. Diagnose-only

Include the Shared language and simplicity block when the user is non-technical.

```text
DIAGNOSE ONLY.

PROBLEM:
<...>

EVIDENCE:
<screenshots/logs/runtime behavior>

DO NOT modify code.
DO NOT reset or destroy data unless inspection absolutely requires it and the user approves.

Inspect:
1. runtime/process/state;
2. logs;
3. relevant database/state;
4. current implementation;
5. project/spec expectations.

Return:
CONFIRMED:
- ...

LIKELY:
- ...

UNKNOWN:
- ...

Then recommend the smallest next action.
Stop before editing.
```

## 6. Manual Smoke Inspection

Include the Shared language and simplicity block when the user is non-technical.

Use when the manual test reveals a discrepancy and it is unclear whether the feature is missing, deferred, or intentionally out of scope.

```text
MANUAL SMOKE INSPECTION — DO NOT MODIFY CODE

EXPECTED:
<...>

ACTUAL:
<...>

EVIDENCE:
<...>

Inspect the current implementation and the accepted spec/design/roadmap.

Answer:
1. Does the capability exist in backend/service/API?
2. Is it exposed in the current user-facing workflow/UI if required?
3. What exact code/spec evidence supports the answer?
4. Classify the discrepancy as exactly one of:
   A. INTENTIONAL SCOPE
   B. DOCUMENTED DEFERRED
   C. IMPLEMENTATION GAP
5. If it is an implementation gap, identify the smallest correct Fix Round scope.

Do not create a workaround and call the requirement satisfied.
Do not edit files.
Do not commit/tag/push.
```

## 7. Confirmed Fix Round `/plan`

Include the Shared language and simplicity block when the user is non-technical.

```text
/plan

[FINAL/FOCUSED FIX ROUND]

A real blocker/gap has been confirmed during QA/review.

CONFIRMED ROOT CAUSE / GAP:
<...>

REQUIRED FIX:
<...>

PRESERVE:
<already accepted behavior>

NON-GOALS:
<...>

PLAN ONLY.

Return:
A. Existing contract/data flow
B. Exact insertion/fix point
C. Smallest safe change
D. Tests/regressions
E. Files expected to change
F. Risks

DO NOT modify code.
DO NOT commit/tag/push.
Stop for review before `/goal`.
```

## 8. Confirmed Fix Round `/goal`

Include the Shared language and simplicity block when the user is non-technical.

```text
/goal

Implement the approved focused Fix Round plan.

ROOT CAUSE:
<confirmed>

SCOPE:
<...>

PRESERVE:
<...>

ACCEPTANCE:
<...>

Add a regression test that proves the bug/gap cannot silently return when practical.

Run focused tests + directly relevant regression tests + build/smoke as applicable + git diff --check.

DO NOT commit/tag/push.
Stop for manual retest of the failed acceptance step.
```

## 9. Final Regression Review `/plan`

Include the Shared language and simplicity block when the user is non-technical.

```text
/plan

PHASE [N] — FINAL REGRESSION REVIEW BEFORE CHECKPOINT

PREVIOUS STABLE CHECKPOINT:
<tag/commit>

CURRENT STATE:
All planned Phase Rounds have passed user acceptance. The working tree contains the complete uncommitted/uncheckpointed Phase work.

GOAL:
Independently review the full diff from the previous stable checkpoint to the current working tree.

REVIEW ONLY.
DO NOT modify code.
DO NOT commit/tag/push.
DO NOT start the next Phase.

Review for:
- regressions in previous capabilities;
- duplicated or competing sources of truth;
- data/schema/reconciliation safety;
- rollback/atomicity/idempotency/orphan prevention;
- state/persistence/resume errors;
- stale async/race bugs;
- destructive filesystem risk;
- security/auth risk where relevant;
- manual-edit/user-data preservation;
- performance problems that affect accepted behavior;
- missing tests;
- unnecessary complexity that threatens correctness/stability.

Do not request refactors merely for style/elegance.

Return only:
A. Confirmed-correct areas
B. Blocking issues
C. Non-blocking observations
D. Exact final verification tests/smokes
E. Verdict:
   READY FOR FINAL VERIFICATION
   or
   MUST FIX BEFORE CHECKPOINT
```

## 10. Final Verification `/goal`

Include the Shared language and simplicity block when the user is non-technical.

Use only after Final Regression Review has no blocker.

```text
/goal

PHASE [N] — FINAL VERIFICATION

No implementation blocker remains from the independent final regression review.

DO NOT add features.
DO NOT refactor unrelated code.
DO NOT commit/tag/push yet unless the user has already completed final manual acceptance and explicitly authorized checkpointing.

Run the approved final verification set:
- full relevant automated test suites;
- production build;
- launcher/runtime smoke where applicable;
- Phase-specific high-risk regressions;
- git diff --check;
- git status inspection.

Report exact PASS/FAIL evidence and then STOP for final manual smoke.
```

## 11. Final Delta Verification

Include the Shared language and simplicity block when the user is non-technical.

Use after a narrow post-review fix.

```text
FINAL DELTA VERIFICATION

A narrow Fix Round changed the working tree after Final Regression Review.

Review and verify only the delta and its direct regression surface unless new evidence indicates broader risk.

Verify:
- changed files/behavior;
- failed manual-smoke step;
- direct dependencies;
- focused + relevant regression tests;
- build/smoke as applicable;
- git diff --check.

If the change unexpectedly affects schema, canonical data, persistence/recovery, concurrency/job lifecycle, destructive filesystem behavior, security, or broad architecture, STOP and recommend a full Final Regression Review again.

Do not commit/tag/push.
```

## 12. Checkpoint

Include the Shared language and simplicity block when the user is non-technical.

```text
FINAL MANUAL ACCEPTANCE: PASS

Create stable checkpoint:
<TAG>

Before any Git write:
1. run the required final tests/build/smoke;
2. run git diff --check;
3. if anything fails, STOP without commit/tag/push.

If everything passes:
4. update checkpoint/release documentation;
5. commit the accepted Phase changes;
6. create the exact tag;
7. push the intended branch;
8. push the exact tag;
9. verify remote branch contains the commit;
10. verify remote tag exists and points to the intended commit;
11. verify local working tree is clean.

Report commit SHA, tag, remote verification, and clean status.
DO NOT start the next Phase.
```
