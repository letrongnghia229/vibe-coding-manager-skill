# Review Checklists

## Table of contents

1. `/plan` review
2. `/goal` review
3. Implementation review
4. Manual QA
5. Visual QA
6. Spec traceability / gap classification
7. Final Regression Review
8. Final Verification readiness
9. Delta Verification scope
10. Checkpoint readiness
11. Token/efficiency sanity check
12. User Understanding / Complexity Escalation check

## 1. `/plan` review

Check:
- Does the plan identify a plausible root cause supported by repository evidence?
- Is it fixing the canonical/root-cause layer rather than a visible symptom only?
- Does it create a second source of truth or duplicate tree/state/model?
- Is the scope bounded to the Round?
- Are non-goals respected?
- Are migrations/data changes really necessary?
- Is backward compatibility addressed?
- Are atomicity/rollback/idempotency/orphan risks addressed when persistent data changes?
- Are tests proportional to the change?
- Is manual acceptance testable?
- For UI: does the plan follow the approved Master/UI spec without inventing a new direction?
- Has the proposed solution become materially more complex than the original problem?
- If yes, has a simpler product/UX/configuration/workflow constraint been reconsidered before approving new architecture?

Verdict: `PASS` or `REVISE`.

If the review contains a complex/high-risk blocker or architecture change, run section 12 before producing the next Codex handoff.

## 2. `/goal` review

Before giving `/goal` to Codex, verify:
- it references the approved plan from the same session instead of restating it;
- only review deltas/critical guardrails are repeated;
- verification is proportionate to Round risk;
- explicit no commit/tag/push before acceptance is present when applicable;
- stop condition for manual QA is clear;
- if the plan session was lost, only a compact approved-plan summary is included;
- any required User Understanding Gate has already been satisfied;
- a high-risk `/goal` asks Codex to use the non-technical reporting block if it encounters a blocker, STOP, or invalidated assumption.

For UI Rounds also include:
- Master/page spec source of truth;
- no unauthorized redesign;
- Visual QA requirement.

## 3. Implementation review

Check evidence for:
- files actually changed;
- expected behavior actually implemented;
- tests run and exact results;
- build/smoke status;
- migrations/schema/data changes;
- known limitations;
- unexpected extra scope;
- blocker/STOP/invalidated-assumption evidence that requires explanation before another implementation attempt.

Do not equate "Codex says done" with acceptance.

If implementation unexpectedly opens a larger architecture problem, do not immediately generate another `/goal`; run section 12 first.

## 4. Manual QA

Prefer observable user steps:
1. starting state;
2. action;
3. expected visible/resulting state;
4. persistence/restart check if relevant;
5. failure/error state if relevant.

Track which steps are PASS/FAIL/PENDING so a later Fix Round can resume the smoke test intelligently.

## 5. Visual Q@

Review screenshots/renders for:
- information hierarchy;
- alignment/grid;
- spacing rhythm;
- text size/line-height/measure;
- contrast and semantic color use;
- button/input/card/menu/tab consistency;
- icon consistency;
- viewport fit;
- vertical/horizontal overflow;
- scroll ownership;
- responsive layout;
- selected/hover/focus/disabled states;
- loading/empty/error states;
- obvious accessibility failures.

Compare to approved Master/page spec.

Classify:
- MUST FIX: requirement/accessibility/usability/regression failure;
- SHOULD FIX: clear quality problem that should be addressed in the current UI modernization scope;
- OPTIONAL: polish that can safely wait.

## 6. Spec traceability / gap classification

When manual behavior is missing:

Trace:
`accepted requirement -> design/spec -> implementation -> user-facing workflow -> tests`

Classify exactly:
- INTENTIONAL SCOPE: not required now;
- DOCUMENTED DEFERRED: explicitly moved later;
- IMPLEMENTATION GAP: accepted requirement missing from implementation.

A workaround is not acceptance if the required user workflow is absent.

## 7. Final Regression Review

Review previous stable checkpoint -> current working tree for:
- previous-Phase regressions;
- canonical data/model consistency;
- migration safety;
- transaction atomicity;
- rollback/recovery;
- idempotency;
- orphan prevention;
- state/progress/resume persistence;
- stale request/race issues;
- destructive source/filesystem safety;
- manual-edit/user-data preservation;
- security/auth where applicable;
- UI/UX acceptance regressions;
- test coverage;
- complexity that threatens stability.

Do not block for cosmetic code style.

For every HIGH/CRITICAL blocker, also record:
- practical user impact;
- whether damage is confirmed or only possible;
- why it blocks checkpoint;
- whether the fix is still small or has escalated into an architecture decision.

Before creating the Fix Round prompt, run section 12 when the blocker is complex/high-risk.

## 8. Final Verification readiness

Must have:
- no unresolved blocker from final review;
- all normal/Fix Rounds accepted;
- exact final test/smoke plan;
- stable previous checkpoint known;
- current working tree understood.

## 9. Delta Verification scope

Delta verification is acceptable only if:
- fix is narrow;
- direct regression surface is known;
- no schema/canonical-data/persistence/recovery/security/concurrency/destructive behavior changed.

Otherwise rerun broader final review.

## 10. Checkpoint readiness

Before authorizing checkpoint:
- final review clean;
- final automated verification PASS;
- final manual smoke PASS;
- Visual QA PASS for UI Phase scope;
- docs ready;
- intended tag name known;
- no blocker/deferred ambiguity.

Checkpoint completion requires:
- commit;
- tag;
- push branch;
- push tag;
- remote verification;
- clean local working tree.

## 11. Token/efficiency sanity check

Before asking for another Codex call, review, or test cycle, ask:
- Will this produce new evidence or only repeat already-fresh PASS evidence?
- Can stable context be referenced from `AGENTS.md`/project docs instead of pasted again?
- Can the prompt carry a delta instead of the full previous plan/report?
- Is the selected model/effort the cheapest safe choice for the actual risk?
- Can focused tests answer the question before a full suite?
- Is an independent reviewer actually justified by risk?
- Is the user being asked to relay another technical prompt before understanding why it is necessary?

Never skip data-safety, migration, recovery, security, or required acceptance checks merely to save tokens.

## 12. User Understanding / Complexity Escalation check

Run this before the next Codex implementation handoff when the current finding is complex/high-risk or the user appears not to understand it.

### Understanding checklist

- [ ] Can I explain the root problem in 1–3 plain Vietnamese sentences?
- [ ] Did I give at least one concrete everyday analogy/example for a complex mechanism?
- [ ] Did I state severity in practical terms?
- [ ] Did I distinguish potential risk from confirmed damage?
- [ ] Did I explain why the obvious fix is insufficient, if relevant?
- [ ] Does the user understand why Codex stopped or why the plan must change?
- [ ] If a meaningful trade-off exists, did I present at most 2–3 options and recommend one?
- [ ] Did I avoid asking the user to choose low-level implementation details that have no product trade-off?
- [ ] If a simpler product/workflow rule might solve the root issue, did I give the user a chance to propose it?

### Complexity Escalation checklist

Trigger when the solution path grows materially, for example:

`simple validation -> DB edge case -> process lock -> native dependency/custom subsystem`

Check:

- [ ] What was the original problem in one sentence?
- [ ] What new complexity has been added?
- [ ] Is the added complexity necessary to preserve an accepted requirement, or only to preserve an implementation choice?
- [ ] Could a product/UX/configuration/workflow constraint eliminate the root problem more simply?
- [ ] Would that simpler constraint still satisfy data safety, compatibility, and user workflow requirements?
- [ ] If the complex design remains necessary, has the user been told the maintenance/dependency cost in plain language?

### Handoff rule

Only after the required explanation/decision gate is satisfied should ChatGPT generate the next Codex `/plan`, `/goal`, or diagnose prompt.

Use `nontechnical-communication.md` for the full protocol.
