# Workflow State Machine

## Top-level flow

```text
IDEA -> BRAINSTORM -> CRITIQUE -> FREEZE SPEC
  -> PHASE / ROUND PLANNING
  -> UI DESIGN GATE? (conditional)
  -> MODEL SELECTION -> NEW CODEX SESSION
  -> /plan -> ChatGPT review
       -> complex/high-risk finding? USER UNDERSTANDING GATE
       -> REVISE: correction delta -> /plan
       -> PASS: /goal
  -> implementation -> automated verification -> Codex report
       -> blocker / STOP / invalid assumption?
          YES: USER UNDERSTANDING GATE -> diagnose / re-plan / Fix Round
          NO: manual QA
  -> UI Round? VISUAL QA
  -> ROUND_ACCEPTED
  -> more Rounds? next Round
  -> all Rounds accepted: independent FINAL REGRESSION REVIEW
       -> high-risk blocker? USER UNDERSTANDING GATE -> Fix Round
       -> clean: FINAL VERIFICATION -> FINAL MANUAL SMOKE
  -> checkpoint -> commit/tag/push/verify -> NEXT PHASE only when user asks
```

`USER UNDERSTANDING GATE` and `COMPLEXITY ESCALATION CHECK` are conditional overlays, not extra ceremony for routine work.

## Stage rules

### Brainstorm / critique / freeze

Before coding:
- clarify the problem, user workflow, must-haves, non-goals, risks, and simplest viable approach;
- critique unnecessary complexity and duplicate sources of truth;
- freeze product/architecture decisions before implementation planning;
- for a non-technical user, discuss product/workflow meaning before internal implementation detail.

If a proposed solution is much more complex than the original problem, run the Complexity Escalation Check before freezing architecture.

### Phase / Round planning

Prefer vertical, testable slices. One Round should normally have:
- one major goal;
- explicit non-goals;
- one Codex session/model;
- focused automated verification;
- observable manual acceptance.

Use `docs/CURRENT_PHASE.md` when present as a short active-state ledger. Do not turn `CHECKPOINTS.md` into a per-Round progress log.

### UI Design Gate

Trigger for meaningful new pages/layout/navigation/interaction/design-system/responsive work.

Flow:

`functional need -> UI proposal -> critique -> user approval -> freeze UI spec -> Round`

Skip for tiny cosmetic corrections. After implementation, require Visual QA against the approved Master/UI spec.

### Model selection

Choose the cheapest safe model for the whole Round. See `model-selection.md`. Do not switch mid-session casually; if risk class materially rises, stop and restart the Round session with the appropriate model.

### `/plan`

Use `/plan` for non-trivial work. It is inspect/plan only.

Prompt Codex to:
- read `AGENTS.md`, `docs/CURRENT_PHASE.md` if present, and only relevant code/docs;
- identify root cause/current flow;
- propose the smallest safe change;
- list affected files, risks, compatibility/migration implications, tests;
- stop without editing/commit/tag/push.

For blocker/STOP/architecture/data-safety findings, require the non-technical reporting block from `codex-prompts.md`.

A direct `/goal` is acceptable only for a tiny, fully specified, low-risk, mechanical task with no unresolved design/root-cause question.

### Plan review

Return `PASS` or `REVISE`.

Check:
- root cause vs symptom patch;
- canonical authority/data flow;
- scope creep;
- compatibility/migration/data safety;
- regression surface and tests;
- duplicate source of truth;
- whether complexity has escalated materially.

Before another Codex prompt, run the User Understanding Gate when the finding is complex/high-risk or the user would otherwise only copy a technical correction they do not understand.

### `/goal`

Generate only after plan PASS and any required understanding/product decision gate.

In the same Codex session, reference the approved plan instead of repeating it. Carry only review deltas, critical guardrails, focused verification, STOP rules, and no-commit/no-next-Phase rules.

### Implementation review

Do not equate “Codex says done” with acceptance. Review changed behavior, tests/builds, scope, risks, and manual QA needs.

If Codex reports `STOP`, `BLOCKED`, an invalid assumption, or a new architecture dependency, do not immediately issue another implementation prompt. Explain first, then decide whether to diagnose, re-plan, simplify, or create a Fix Round.

### Manual QA / Visual QA

Give short observable steps: starting state -> action -> expected result -> persistence/restart/error checks when relevant.

Track `PASS / FAIL / PENDING`.

For UI Rounds, also review hierarchy, spacing, typography, contrast, components, overflow/scroll, responsive states, loading/empty/error states, focus/accessibility, and visual regressions.

### Diagnose / Fix Round

If root cause is unknown, diagnose before editing and separate `CONFIRMED / LIKELY / UNKNOWN`.

For a confirmed blocker, use a focused Fix Round. Use `/plan` first when uncertainty remains.

If the Fix Round unexpectedly expands into a new subsystem/native dependency/major architecture, pause and run the Complexity Escalation Check before further implementation.

### Round accepted

A Round is accepted only when required automated verification, manual QA, and Visual QA (if applicable) pass and no blocker remains.

Mark `ROUND_ACCEPTED` in the active ledger if used. Do not automatically commit/tag every Round when the project defers checkpoints until Phase end.

### Final Regression Review

After all planned Rounds are accepted, use a fresh independent high-risk review session.

Review previous stable checkpoint -> current local worktree for:
- regressions;
- canonical data/model consistency;
- migration/persistence/recovery safety;
- race/concurrency/orphan issues;
- destructive filesystem/source safety;
- security where relevant;
- user-data/manual-edit preservation;
- missing critical tests;
- complexity that threatens stability.

Verdict: `READY FOR FINAL VERIFICATION` or `MUST FIX BEFORE CHECKPOINT`.

For HIGH/CRITICAL blockers, run the User Understanding Gate before the Fix Round handoff.

### Final Verification / Manual Smoke / checkpoint

Run a comprehensive but proportionate final test/build/smoke set, then a short final manual smoke.

If a post-review fix changes migration, canonical data, persistence/recovery, concurrency, destructive behavior, security, or broad architecture, rerun broad final review rather than delta-only verification.

Checkpoint only after final manual PASS:

`tests -> build -> smoke -> docs -> commit -> tag -> push branch -> push tag -> verify remote -> clean working tree`

Never start the next Phase automatically.

## User Understanding Gate

Trigger when:
- Codex reports a blocker or STOP condition;
- an approved assumption is disproved;
- the issue affects data safety, recovery, migration, security, destructive behavior, concurrency, or process lifetime;
- the solution adds a substantial dependency/native component/new canonical boundary;
- the user says the explanation is too technical;
- the next action requires a meaningful user decision.

Before the next implementation prompt, ChatGPT must cover:

1. **Chuyện gì đang xảy ra?** — 1–3 plain Vietnamese sentences.
2. **Ví dụ dễ hiểu** — at least one concrete analogy/example for a complex mechanism.
3. **Mức độ nghiêm trọng** — LOW / MEDIUM / HIGH / CRITICAL plus practical worst case.
4. **Thiệt hại đã xảy ra chưa?** — confirmed damage / potential risk / unknown.
5. **Vì sao cách sửa hiển nhiên chưa đủ?** — when relevant.
6. **Lựa chọn** — at most 2–3 practical options when user input matters.
7. **Khuyến nghị** — one recommended direction.
8. **User Idea Window** — when a simpler product/workflow constraint may exist, let the user propose it before architecture is frozen.

Technical detail may follow as a second layer. It must not replace the plain-language layer.

## Complexity Escalation Check

Trigger when a small problem starts producing a much larger solution, for example:

```text
simple validation -> database edge case -> process lock -> native dependency -> custom subsystem
```

Before approving the larger design:
1. restate the original problem in one plain sentence;
2. state how solution complexity grew;
3. revisit whether a product/UX/configuration/workflow constraint can eliminate the root issue while preserving accepted safety requirements;
4. compare simple vs complex paths on user impact, safety, reversibility, maintenance, and compatibility;
5. only approve the larger architecture if simpler paths fail accepted requirements.

This is not permission to weaken data safety.

## Anti-pattern

```text
Codex technical report
-> ChatGPT says PASS/REVISE
-> ChatGPT gives another technical prompt
-> user copies it back without understanding why
```

For a non-technical user, that is workflow failure.

Preferred:

```text
Codex evidence -> ChatGPT verifies -> explains simply -> user can question/propose -> decision freezes -> Codex executes
```

See `nontechnical-communication.md` for the detailed explanation protocol.
