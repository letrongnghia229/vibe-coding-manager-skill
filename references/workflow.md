# Workflow State Machine

## Table of contents

1. Top-level flow
2. Brainstorm
3. Critique
4. Freeze spec
5. Phase planning
6. Round planning
7. UI Design Gate
8. Model selection
9. `/plan`
10. Plan review
11. `/goal`
12. Implementation review
13. Manual QA
14. Visual QA
15. Diagnose / inspection
16. Fix Round
17. Round accepted
18. Final Regression Review
19. Final Verification
20. Final Manual Smoke
21. Final Delta Verification vs full re-review
22. Checkpoint
23. Next Phase


## 1. Top-level flow

```text
IDEA
 ↓
BRAINSTORM
 ↓
CRITIQUE
 ↓
FREEZE SPEC
 ↓
PHASE PLANNING
 ↓
ROUND PLANNING
 ↓
UI DESIGN GATE? ── no ──────────────┐
 ↓ yes                               │
UI DESIGN / CRITIQUE                 │
 ↓                                   │
USER APPROVAL                        │
 ↓                                   │
FREEZE UI SPEC                       │
 └───────────────────────────────────┘
 ↓
MODEL SELECTION
 ↓
NEW CODEX SESSION
 ↓
/plan
 ↓
PLAN REVIEW
 ├─ REVISE → return to /plan
 └─ PASS
      ↓
     /goal
      ↓
 IMPLEMENTATION
      ↓
 AUTOMATED VERIFICATION
      ↓
 MANUAL QA
      ↓
 UI ROUND? → VISUAL QA
      ↓
 ├─ FAIL → DIAGNOSE/FIX → TEST AGAIN
 └─ PASS → ROUND ACCEPTED
              ↓
          MORE ROUNDS?
          ├─ YES → NEXT ROUND
          └─ NO
               ↓
        FINAL REGRESSION REVIEW
               ↓
        FINAL VERIFICATION
               ↓
         FINAL MANUAL SMOKE
               ↓
        ├─ FAIL → INSPECT → FIX ROUND → RETEST → DELTA/FULL REVIEW
        └─ PASS
               ↓
           CHECKPOINT
               ↓
           NEXT PHASE
```

## 2. Brainstorm

Use when the user has an idea, problem, or feature concept that is not yet frozen.

Produce:
- problem statement;
- target user/workflow;
- must-have / should-have / nice-to-have / future;
- risks and unknowns;
- simplest viable approach;
- non-goals.

Do not generate an implementation `/goal` while product direction is still open.

## 3. Critique

Use a skeptical reviewer mindset. Evaluate:
- correctness;
- unnecessary complexity;
- hidden dependency/risk;
- maintainability;
- whether the proposal is suitable for AI-assisted implementation;
- whether a simpler solution exists.

Prefer 1–2 critique rounds, then freeze decisions instead of endlessly reopening scope.

## 4. Freeze spec

Convert approved decisions into explicit source-of-truth requirements:
- goal;
- workflow;
- functional requirements;
- non-functional requirements;
- constraints;
- non-goals;
- acceptance criteria;
- known risks;
- later/deferred items.

For substantial UI work, functional spec may be frozen before the UI Design Gate; the UI spec is frozen after the visual/interaction direction is approved.

## 5. Phase planning

Create vertical slices. Each Phase should deliver a testable capability rather than only infrastructure.

For every Phase define:
- goal;
- scope;
- non-goals;
- deliverables;
- automated verification;
- manual acceptance;
- intended stable checkpoint.

Avoid building future infrastructure before a current capability needs it.

## 6. Round planning

Split a Phase when it contains multiple coupled risks or unrelated changes.

Good Round properties:
- one major goal;
- small enough for one Codex session;
- independently reviewable/testable;
- explicit non-goals;
- minimal cross-subsystem churn.

Avoid combining unrelated backend/data/UI refactors into one Round.

## 7. UI Design Gate

Trigger for meaningful changes to:
- new pages/screens;
- layout or information hierarchy;
- navigation;
- major interaction flows;
- design system;
- responsive behavior;
- broad visual modernization.

Skip for tiny isolated changes such as a typo, one icon, or a small spacing correction.

Flow:

`functional need -> inspect existing Master -> UI proposal -> critique -> user approval -> freeze UI spec -> Codex Round`

If there is no approved Master, create a design-system direction first. If the app already exists with developer-looking UI, follow the retrofit path in `ui-ux-integration.md`.

## 8. Model selection

Select one model for the full Round before opening the Codex session. See `model-selection.md`.

## 9. `/plan`

For non-trivial work, `/plan` is the repository-inspection stage. Keep the prompt compact and tell Codex to read `AGENTS.md` plus only the relevant docs/files.

The prompt should request only what matters to the Round:
- root cause/current flow;
- exact files/functions/components;
- migration/backward-compatibility implications when relevant;
- risk/regression surface;
- tests;
- smallest safe implementation plan.

Require PLAN ONLY, no edits/commit/tag/push.

A direct `/goal` may skip `/plan` only when the task is fully specified, tiny/mechanical, low-risk, contains no unresolved design/root-cause question, and is easy to verify by a narrow diff/test. Documentation-only or obvious copy/rename tasks are typical examples.

## 10. Plan review

Return:
- `PASS`, or
- `REVISE` with precise changes.

When a plan needs revision, prefer a correction delta instead of asking Codex to restate the entire plan. Only request a full rewritten plan when the original plan is structurally invalid or the session lost context.

Do not generate `/goal` on a plan that still has unresolved root-cause, scope, or data-safety problems.

## 11. `/goal`

Generate `/goal` only after plan approval.

In the same Codex session, do **not** restate the approved plan. Reference it and carry only:
- review corrections/deltas or critical guardrails added after plan review;
- proportionate verification expectations;
- conflict/stop conditions;
- no-commit/no-next-Round rules when applicable.

If the `/plan` session was lost or intentionally restarted, provide a compact approved-plan summary rather than the full transcript.

Use the same Codex session/model as `/plan` unless the original session is intentionally abandoned.

## 12. Implementation review

When Codex reports completion:
- inspect what changed conceptually;
- check tests/build/smoke evidence;
- identify manual acceptance steps;
- do not treat Codex self-report as user acceptance.

Use independent implementation review only when risk justifies it (for example migration/persistence/recovery/concurrency/security/destructive behavior or a high-risk cross-module change). Do not require a second reviewer for every small isolated Round.

Reuse fresh verification evidence when no code changed; do not request another identical test cycle merely to repeat PASS evidence.

## 13. Manual QA

Give the user a short, concrete checklist focused on observable behavior. Prefer 3–10 steps for a Round.

Track acceptance state:
- NOT_TESTED
- IN_PROGRESS
- BLOCKED
- FIX_REQUIRED
- ROUND_ACCEPTED
- READY_FOR_FINAL_REVIEW
- READY_FOR_FINAL_VERIFICATION
- READY_FOR_CHECKPOINT
- CHECKPOINTED

## 14. Visual QA

For UI Rounds, manual QA is not enough. Also inspect screenshots or rendered states for:
- hierarchy;
- alignment;
- spacing;
- typography;
- color/contrast;
- component consistency;
- density;
- overflow/scroll;
- responsive behavior;
- empty/loading/error states;
- keyboard/focus/accessibility where relevant.

Compare against the approved Master/UI spec rather than subjective "make it prettier" criteria.

## 15. Diagnose / inspection

Use when the root cause is unknown or evidence contradicts assumptions.

Require inspection only before edits. Separate:
- CONFIRMED
- LIKELY
- UNKNOWN

For manual-smoke discrepancies, classify:
- INTENTIONAL SCOPE
- DOCUMENTED DEFERRED
- IMPLEMENTATION GAP

A backend/API workaround may prove a capability exists, but it does not satisfy a missing user-facing requirement if the spec requires UI exposure.

## 16. Fix Round

When a blocker is confirmed, prefer a fresh focused Codex session rather than continuing a review-only session.

Select the Fix Round model by current risk using `model-selection.md`; do not default to Extra High.

Use `/plan` first if the fix has meaningful uncertainty; a fully specified tiny low-risk correction may go directly to `/goal`.

## 17. Round accepted

A Round is accepted only when:
- required automated verification passes;
- user manual acceptance passes;
- visual QA passes for UI Rounds;
- no known blocker remains.

Do not automatically commit/tag after every Round unless the project explicitly uses per-Round checkpoints.

## 18. Final Regression Review

Trigger after all planned Rounds of a Phase are accepted.

Use a fresh independent Codex session, normally Sol High. Reserve Extra High for unusually high-risk/uncertain Phase diffs.

Review the diff from the previous stable checkpoint to current local working tree.

Look for:
- regression in previous capabilities;
- duplicated sources of truth;
- unsafe migration/reconciliation;
- state/persistence errors;
- race/stale-request bugs;
- destructive filesystem risk;
- rollback/idempotency/orphan problems;
- manual-edit preservation regressions;
- unnecessary complexity that affects correctness/stability;
- missing test coverage.

Do not refactor for style.

Verdict:
- `READY FOR FINAL VERIFICATION`, or
- `MUST FIX BEFORE CHECKPOINT`.

## 19. Final Verification

Run a comprehensive but proportionate verification set:
- backend/frontend tests as relevant;
- production build;
- launcher/smoke tests;
- high-risk Phase-specific regression tests;
- `git diff --check`;
- working-tree inspection.

Do not commit/tag yet if final manual smoke is still pending.

## 20. Final Manual Smoke

Use a short 5–12 step list of the highest-value workflows and high-risk regressions.

If a step fails:
1. mark checkpoint readiness invalid;
2. inspect before guessing;
3. classify the gap;
4. create a Fix Round if required;
5. retest the failed step;
6. resume remaining smoke steps where safe.

## 21. Final Delta Verification vs full re-review

After a post-review fix:

Use **Final Delta Verification** when the fix is narrow and low-risk:
- few files;
- no migration;
- no canonical-data change;
- no worker/concurrency/security/destructive behavior;
- direct regression coverage exists.

Rerun broader Final Regression Review when the fix changes:
- schema/migration;
- canonical data or reconciliation;
- persistence/recovery;
- worker/job lifecycle/concurrency;
- destructive filesystem behavior;
- security/auth;
- broad cross-module architecture.

## 22. Checkpoint

Checkpoint only after final manual PASS.

Expected sequence:

`tests -> build -> smoke -> docs -> commit -> tag -> push branch -> push tag -> verify remote -> clean working tree`

If verification fails, stop before declaring checkpoint complete.

## 23. Next Phase

Only begin after the stable remote checkpoint exists and the user asks to continue.
