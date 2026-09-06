# Codex Token & Credit Efficiency

## Table of contents

1. Goal
2. Stable project rules in `AGENTS.md`
3. Three-layer context model
4. One Round = one session = one model
5. `/goal` does not repeat `/plan`
6. Compact `/plan`
7. Narrow inspection
8. Progressive verification
9. Proportionate review
10. Compact Codex output
11. Risk-based model and effort
12. Cache-friendly habits
13. Avoid expensive parallelism
14. Keep tools minimal
15. Natural compaction
16. Monitor usage
17. ChatGPT manager behavior
18. Keep product decisions out of Codex when repo work is unnecessary
19. Compact default handoff templates

## Goal

Reduce Codex usage without weakening correctness, data safety, or acceptance gates.

Use this priority order:

1. avoid unnecessary work;
2. avoid repeated context;
3. choose the cheapest model/effort that safely fits the Round;
4. run proportionate tests/reviews;
5. keep prompts and reports compact;
6. preserve cache-friendly session stability where practical.

Do not optimize token usage by skipping a safety-critical inspection, migration check, recovery test, or required acceptance step.

## 1. Put stable project rules in `AGENTS.md`

Prefer a short repository-root `AGENTS.md` for durable instructions Codex needs across Rounds:

- source-of-truth docs;
- repository safety rules;
- language/reporting preference;
- build/test entry points;
- stable product constraints;
- Git policy.

Keep active Round status, temporary bugs, current acceptance evidence, and long product specs out of `AGENTS.md`; those become stale and waste context.

A good `AGENTS.md` should point to project docs instead of copying them.

## 2. Use a three-layer context model

```text
AGENTS.md = stable repository rules
Project docs = detailed WHAT / architecture / specs
Current prompt = only NOW / delta / evidence
```

Do not paste full PRDs, roadmaps, previous plans, or old reports into every Codex prompt when Codex can read the repository versions.

## 3. One Round = one Codex session = one model

Default:

`/plan -> ChatGPT review -> /goal -> implementation -> focused verification -> report`

Keep the same Codex session and model for the Round so the approved plan remains in context.

Open a new Codex session when:

- starting a new Round;
- deliberately requesting an independent review;
- abandoning a compromised/confused session;
- risk classification rises enough to require a stronger model.

Do not open a new session merely between `/plan` and `/goal`.

## 4. `/goal` must not repeat `/plan`

After a plan passes, default `/goal` shape:

```text
/goal

[ROUND NAME]

Implement the approved plan from this session.

Review deltas / guardrails:
- <only corrections added during plan review>

Verification:
- <focused tests appropriate to this Round>

Report concisely in <language>.
Do not restate the plan.
Do not commit/tag/push.
Do not start the next Round/Phase.
```

Repeat plan details only if:

- the Codex session was lost/restarted;
- the plan was external to the current session;
- a critical requirement would otherwise be ambiguous.

Even then, provide a compact approved-plan summary, not the full transcript.

## 5. Make `/plan` prompts compact

A normal `/plan` usually needs only:

- Round name;
- one-sentence context;
- current problem/goal;
- scope/non-goals that are not already in `AGENTS.md`/docs;
- acceptance criteria;
- relevant evidence;
- explicit PLAN ONLY stop rule.

Tell Codex to read `AGENTS.md` and only the relevant project docs/files. Avoid telling it to read the entire repository unless broad inspection is genuinely required.

## 6. Inspect narrowly before broadly

Prefer:

- `git status` / `git diff` / `git diff --stat`;
- targeted `rg`/search;
- named files/functions/modules;
- relevant migration/tests;

before broad repository-wide reading.

Expand only when evidence indicates the issue spans more areas.

## 7. Use progressive verification

During implementation:

1. run the smallest focused test that proves the changed behavior;
2. run directly relevant regression tests;
3. run broader/full suites only at the appropriate risk/acceptance gate.

Default verification budget:

- **Low risk**: focused tests + `git diff --check` (and a narrow build/lint only if directly relevant).
- **Medium risk**: focused tests + directly relevant regression tests + build/smoke when the changed surface requires it.
- **High risk**: focused + broad affected regression/safety tests; include migration/recovery/concurrency/security checks when relevant.
- **Final Phase gate**: comprehensive but proportionate final verification across the affected product surface.

Avoid repeatedly running the full suite after every small edit.

Do not run an extra verification cycle after manual QA if:

- no code changed since the last automated verification;
- no new blocker was discovered;
- the Round already passed the required automated + manual gates.

After a narrow fix, use delta verification when safe. Re-run broad review/testing for migration, persistence, recovery, concurrency, security, destructive filesystem, or broad architecture changes.

## 8. Review proportionately

Use independent implementation review when its value justifies the cost, especially for:

- migrations/data safety;
- recovery/restore;
- concurrency/job lifecycle;
- destructive filesystem operations;
- auth/security;
- broad architecture;
- final Phase regression.

Do not automatically require an independent reviewer for every small isolated feature/fix.

## 9. Keep Codex output compact

Ask for short reports by default:

- files changed;
- behavior changed;
- exact test/build result;
- blocker/known risk;
- manual QA steps.

Do not request:

- full diff pasted into chat;
- large unchanged code blocks;
- restatement of the approved plan;
- verbose test logs when all tests pass.

If a command fails, ask for the relevant error excerpt, not the entire log unless needed.

## 10. Choose model and reasoning effort by risk

Use the cheapest safe combination for the whole Round.

Suggested ladder:

- **Luna**: tiny, isolated, mechanical, easy to verify.
- **Terra Medium/High**: normal features, API wiring, UI, bounded bug fixes.
- **Sol High**: migrations, data safety, recovery/restore, concurrency, difficult diagnosis, independent high-risk review.
- **Sol Extra High**: unusually high failure cost, ambiguous architecture, destructive recovery, or a problem that Sol High is not resolving reliably.

Do not default to Extra High merely because a task uses `/plan` or `/goal`.

If a lower-risk session reveals substantially higher risk, stop and reopen the Round with the stronger model rather than switching casually mid-session.

## 11. Cache-friendly habits

Prompt caching is automatic; do not build product complexity around it.

To improve the chance of cache hits:

- keep stable instructions at the beginning and variable evidence later;
- keep model, working directory, sandbox/approval mode, and tool set stable within a Round when practical;
- avoid rewriting/reordering the same long static prefix every turn;
- keep the same session for the Round.

Do not sacrifice clear prompts or safety just to chase cache hits.

## 12. Avoid expensive parallelism by default

Do not use Best-of-N, multiple agents, broad sub-agent delegation, or duplicated independent analyses for routine work.

Use them only when the expected quality/risk benefit is worth multiple model runs.

## 13. Keep tools minimal

Use repository inspection first for repository questions.

Do not add MCPs/connectors/web research to a Codex task unless they materially help the current goal. Extra tools can enlarge context and change the prompt/tool prefix.

## 14. Let Codex compact naturally

Long sessions may be compacted automatically. Do not manually paste giant conversation summaries back into the same active Round unless Codex actually lost necessary context.

When a Round becomes confused or context-heavy, prefer ending it at a clean boundary and starting the next Round in a new session.

## 15. Monitor usage

When the user cares about Codex consumption:

- recommend checking the Codex/Work usage indicator or usage dashboard;
- in Codex CLI, use `/status` when available;
- compare usage across similar Rounds rather than estimating only by visible prompt length.

Usage depends on model, reasoning effort, context, task complexity, tools, and runtime—not prompt characters alone.

## 16. ChatGPT manager behavior

When generating Codex handoffs:

- do not paste known project rules already present in `AGENTS.md` unless they are critical to the immediate risk;
- reference relevant docs by path;
- carry only review deltas from `/plan` into `/goal`;
- avoid redundant verification suggestions after PASS with no code delta;
- give the user one next action;
- prefer Vietnamese for nontechnical Vietnamese users when requested/project-defined.


## 17. Keep product decisions with ChatGPT/user when repo work is unnecessary

Use ChatGPT + the user for product scope, priority, acceptance, and architecture tradeoff decisions that can be made from already-known project context.

Use Codex when repository inspection, current implementation evidence, code changes, tests, or Git operations are materially required.

Do not spend a high-cost Codex session debating product options that should be frozen before implementation. If a product decision depends on repository facts, ask Codex for a narrow inspection/diagnosis rather than a broad design exercise.

## Compact default handoff templates

### `/plan`

```text
/plan

[ROUND NAME]

Read AGENTS.md and the relevant project docs.

GOAL:
<one concise goal>

EVIDENCE / CURRENT GAP:
<only current evidence>

ACCEPTANCE:
1. ...
2. ...

PLAN ONLY. Inspect the smallest relevant surface first.
Do not modify files. Do not commit/tag/push. Stop for review.
```

### `/goal`

```text
/goal

[ROUND NAME]

Implement the approved plan from this session.

REVIEW DELTAS / GUARDRAILS:
- <only new constraints from plan review, if any>

Run focused + directly relevant regression tests; broaden only if risk requires it.
Report concise results without restating the plan.

Do not commit/tag/push. Do not start the next Round/Phase.
```

### Implementation report request

```text
Report only:
1. files changed;
2. behavior changed;
3. exact tests/builds and PASS/FAIL;
4. deviations/blockers/known risks;
5. manual QA steps.
Do not restate the plan or paste large diffs/logs.
```
