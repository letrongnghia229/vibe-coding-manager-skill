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


## 1. `/plan` review

Check:
- Does the plan identify a plausible root cause supported by repository evidence?
- Is it fixing the canonical/root-cause layer rather than a visible symptom only?
- Does it create a second source of truth or duplicate tree/state/model?
- Is the scope bounded to the Round?
- Are non-goals respected?
- Is the plan solving a current accepted requirement, or a hypothetical future problem?
- What is the minimum viable design that could satisfy the user goal?
- What is the worst realistic consequence of using that simpler design?
- Does the consequence justify added migrations, state machines, schedulers, retry engines, global revision tracking, reconciliation layers, or broad cross-module changes?
- Are migrations/data changes really necessary?
- Is backward compatibility addressed?
- Are atomicity/rollback/idempotency/orphan risks addressed when persistent data changes?
- Are tests proportional to real user risk, rather than primarily validating architecture introduced by the plan itself?
- Is manual acceptance testable?
- For a non-technical user: can the recommendation be explained in plain language, and is one option clearly recommended?
- For UI: does the plan follow the approved Master/UI spec without inventing a new direction?

Verdict: `PASS` or `REVISE`.

A plan may be `REVISE` even when technically correct if its complexity is disproportionate to the V1 consequence being avoided.

## 2. `/goal` review

Before giving `/goal` to Codex, verify it contains:
- approved goal;
- approved plan;
- review guardrails;
- scope/non-goals;
- acceptance criteria;
- regression protections;
- smallest-sufficient-solution guardrail;
- prohibition on speculative future-proof infrastructure;
- the user's language requirement and Codex response-language requirement;
- plain-language summary requirement for non-technical users;
- verification commands/expectations;
- explicit no commit/tag/push before acceptance;
- stop condition for manual QA.

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
- complexity that materially exceeds the approved plan;
- new infrastructure whose value is not tied to an accepted requirement.

Do not equate "Codex says done" with acceptance.

## 4. Manual QA

Prefer observable user steps:
1. starting state;
2. action;
3. expected visible/resulting state;
4. persistence/restart check if relevant;
5. failure/error state if relevant.

Track which steps are PASS/FAIL/PENDING so a later Fix Round can resume the smoke test intelligently.

## 5. Visual QA

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
