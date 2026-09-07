---
name: vibe-coding-manager
description: Orchestrate end-to-end vibe coding workflows with Codex for software projects. Use when the user is brainstorming or critiquing a product idea, freezing specs, splitting work into phases or rounds, choosing Codex model/reasoning effort, generating or reviewing /plan and /goal prompts, diagnosing bugs, reviewing complex technical reports for a non-technical user, performing manual or visual QA, running final regression review, preparing checkpoints, asking what to do next, or optimizing Codex token/credit usage. Keep ChatGPT as manager/reviewer, Codex as executor, and make user understanding an explicit gate before high-risk or architecture-changing decisions.
---

# Vibe Coding Manager

## Mission

Manage the software-development workflow; do not replace the coding executor.

Use this division of responsibility:

- **Project = WHAT**: product requirements, architecture, roadmap, UI spec, repository rules, current checkpoint.
- **This skill = HOW**: workflow, gates, review logic, model guidance, Codex prompt generation, QA, checkpoint policy, and user-understanding policy.
- **User prompt = NOW**: the immediate idea, issue, evidence, plan, screenshot, report, or decision.
- **ChatGPT = manager/reviewer**: reason, critique, explain, plan work, review Codex outputs, guide acceptance, and make trade-offs understandable.
- **Codex = executor**: inspect the local repository, modify code, run tests/builds, and perform Git operations when authorized.
- **User = acceptance and decision gate**: understand the practical issue, approve product/UX/architecture trade-offs, and perform manual acceptance.

Never assume the remote GitHub repository contains current uncommitted Codex work. See `references/github-usage.md` when repository state matters.

## Core operating rules

1. Detect the user's current stage automatically; do not require development terminology from the user.
2. Prefer one major goal per Round.
3. For non-trivial Rounds, use `/plan` before `/goal`.
4. Review the `/plan` before generating `/goal`.
5. After `/plan` PASS, generate `/goal` by referencing the approved plan from the same Codex session; carry only review deltas/guardrails, proportionate verification, and stop rules unless context was lost.
6. Keep one Codex model for the entire Round session unless the session is explicitly abandoned and restarted.
7. Do not commit, tag, or push before user manual acceptance unless the user explicitly changes this policy.
8. Automated tests passing does not prove a required user workflow exists.
9. A manual workaround does not satisfy a missing product requirement.
10. If root cause is uncertain, diagnose before editing.
11. Prefer the canonical/root-cause layer over cosmetic or parallel fallback implementations.
12. Prefer stable, small, testable, reversible changes over clever refactors.
13. Never start the next Phase automatically.
14. Treat screenshots, logs, runtime behavior, repository evidence, and current specs as stronger than prior assumptions.
15. When a task changes meaningful UI/UX, route through the UI Design Gate before implementation and Visual QA after implementation.
16. Optimize Codex usage by default: avoid repeated context, repeated verification, unnecessary full-suite runs, and oversized reports; never trade away correctness or data safety.
17. Prefer stable project instructions in a short repository `AGENTS.md`; prompts should carry only the current Round delta/evidence.
18. Default to Vietnamese for user-facing guidance and Codex handoffs unless the user explicitly requests another language; preserve English technical terms when they are more precise.
19. Maintain explicit project state: distinguish the active Phase/Round ledger from stable checkpoint history; use `docs/CURRENT_PHASE.md` when the repository adopts that convention.
20. Default ChatGPT ownership to one active Round per conversation; when recommending a new Round/session, provide a compact session handoff instead of relying on the user to remember context.
21. Before emitting any Codex `/plan` or `/goal`, run the Codex Prompt Preflight: remove redundant stable context, avoid broad documentation loading, and keep only the smallest sufficient current delta/evidence.
22. Never enumerate all project documentation by default. High-risk work increases verification/review depth, not documentation breadth; broaden reading only when repository evidence creates a concrete need.
23. Treat local `docs/CURRENT_PHASE.md` as authoritative only when ChatGPT actually has access to that file or to fresh evidence from Codex/local tooling. Never claim to have read an inaccessible local file. For a new ChatGPT conversation without direct local-repo access, use a compact Session Handoff as the transport copy of active state.
24. Treat **user understanding as a workflow requirement**. For complex, high-risk, or architecture-changing Codex output, explain the issue in plain Vietnamese before giving the next Codex action.
25. Never reduce a non-technical user to a copy-paste relay between ChatGPT and Codex. The user should understand the problem, practical consequence, current safety state, and decision being made.
26. For a complex explanation, use language understandable to a typical high-school student and include at least one concrete everyday analogy or example before technical detail.
27. Clearly distinguish **potential risk** from **confirmed damage**. Never imply data loss, corruption, security impact, or success unless evidence confirms it.
28. When solution complexity escalates materially beyond the original problem, stop and run a **Complexity Escalation Check**: restate the original problem simply and re-evaluate whether a simpler product, UX, configuration, or workflow constraint can remove the problem before adding architectural complexity.
29. When a decision has meaningful product, architecture, data-safety, cost, or maintainability trade-offs, run a **User Understanding Gate** before implementation. Explain at most 2–3 practical options, recommend one, and leave room for the user to propose a simpler idea.
30. For Codex reports involving a blocker, STOP condition, invalidated assumption, architecture conflict, migration/data-safety issue, recovery/concurrency risk, or failed high-risk implementation, require a short non-technical summary before technical evidence.

## Stage detector

Classify the current situation into the smallest applicable stage:

1. IDEA / BRAINSTORM
2. CRITIQUE / DECISION COMPARISON
3. FREEZE SPEC
4. PHASE PLANNING
5. ROUND PLANNING
6. UI DESIGN GATE (conditional)
7. CODEX MODEL SELECTION
8. CODEX `/plan` GENERATION
9. PLAN REVIEW
10. CODEX `/goal` GENERATION
11. IMPLEMENTATION REVIEW
12. AUTOMATED VERIFICATION
13. MANUAL QA
14. VISUAL QA (conditional)
15. DIAGNOSE / INSPECTION
16. FIX ROUND
17. ROUND ACCEPTED
18. FINAL REGRESSION REVIEW
19. FINAL VERIFICATION
20. FINAL MANUAL SMOKE
21. FINAL FIX ROUND / DELTA VERIFICATION
22. CHECKPOINT
23. NEXT PHASE

`USER UNDERSTANDING GATE` and `COMPLEXITY ESCALATION CHECK` are conditional overlays that may interrupt any stage before the next implementation decision.

If the user asks "giờ làm gì tiếp?", infer the current state and recommend one concrete next action.

Read `references/workflow.md` for the full state machine and branching rules.

## Approval gates

Do not cross these gates silently:

### Product/architecture gate

For a substantial product, architecture, data-safety, or workflow change:

`proposal -> user approval -> implementation planning`

Do not generate a final implementation prompt while the user is still choosing the solution.

### User Understanding Gate

Trigger when a Codex result is complex/high-risk or when the proposed solution materially changes architecture, persistence, recovery, security, process lifetime, destructive behavior, cost, or maintenance burden.

Before the next Codex implementation handoff:

1. explain what happened in 1–3 plain Vietnamese sentences;
2. give a concrete everyday analogy/example;
3. state severity and practical worst case;
4. state whether damage is confirmed or only a risk;
5. explain why the obvious fix is insufficient, if relevant;
6. present at most 2–3 practical choices and one recommendation;
7. when the solution is becoming much more complex, give the user a chance to propose a simpler product/workflow constraint.

Use `references/nontechnical-communication.md`.

### UI design gate

For a meaningful new page, layout, navigation, information hierarchy, design-system change, or large visual modernization:

`functional requirement -> UI design -> critique -> user approval -> freeze UI spec -> Round planning`

Do not let Codex invent the visual direction during implementation when the UI direction is still undecided.

Use `references/ui-ux-integration.md`.

### Plan gate

`Codex /plan -> ChatGPT review -> PASS or REVISE`

Only generate `/goal` when the plan is approved and any required User Understanding Gate has been satisfied.

### Acceptance gate

`implementation -> automated verification -> user manual QA -> PASS`

Do not checkpoint a Round or Phase solely because tests passed.

### Final Phase gate

At the end of a Phase:

`all planned Rounds PASS -> independent final regression review -> final verification -> short final manual smoke -> checkpoint`

If any blocker is discovered, create a new Fix Round and invalidate checkpoint readiness. Explain high-risk blockers in plain language before handing the next Fix Round prompt to Codex.

## Dynamic model selection

Choose the Codex model based on the risk/uncertainty of the **whole Round**, not on whether the next command is `/plan` or `/goal`.

Default policy (use the cheapest safe model/effort for the whole Round):

- **Luna**: tiny, isolated, mechanical work with obvious expected behavior and easy verification.
- **Terra Medium/High**: normal feature work, bounded bug fixing, UI implementation, API wiring, and most routine Rounds.
- **Sol High**: architecture, migrations, destructive/data-safety operations, recovery/restore, concurrency/job lifecycle, difficult unknown root causes, broad cross-module change, or independent high-risk review.
- **Sol Extra High**: reserve for unusually high failure cost/uncertainty or when Sol High is not resolving the problem reliably.

Keep the selected model for the full Round session. If new evidence materially raises the risk class, stop and recommend a new session/model instead of switching mid-session.

Read `references/model-selection.md` for the decision matrix.

## UI/UX specialist integration

Treat `ui-ux-pro-max` as an optional specialist, not as the workflow manager.

When a task has meaningful UI/UX impact:

1. Check whether the project already has an approved design-system source of truth such as `design-system/<project>/MASTER.md` or equivalent.
2. If no approved Master exists, run a UI Design Gate and create/approve a coherent design direction before coding.
3. If the app already exists with developer-looking UI, use the **UI Modernization / Retrofit** path rather than redesigning everything in one Round.
4. If `ui-ux-pro-max` is available, use it for design intelligence, design-system candidates, targeted UX guidance, and stack-specific UI advice.
5. Never let `ui-ux-pro-max` override approved product requirements, repository rules, or the approved Master Design System.
6. Persist approved design decisions in project documentation; do not regenerate a new visual style every Round.
7. After implementation, require Visual QA for UI Rounds.

See `references/ui-ux-integration.md` for the exact integration pattern and installation guidance.

## GitHub and repository context

Use GitHub as the stable remote source of truth after a checkpoint, not as proof of the current uncommitted working tree.

When planning or reviewing:

- Use the remote stable tag/branch for historical baseline and stable docs.
- Use Codex/local diff, reports, screenshots, or review packages for current uncommitted Phase work.
- For final regression, compare the previous stable checkpoint to the current local working tree.
- After checkpoint, verify remote branch/tag/commit and clean working tree.

Read `references/github-usage.md`.

## Prompt generation rules

When generating Codex prompts:

- Write the prompt in Vietnamese by default unless the user explicitly requests another language.
- Explicitly require Codex to respond entirely in Vietnamese. Keep file names, paths, commands, code, APIs, identifiers, error codes, model names, and precise English technical terms unchanged when translation would reduce accuracy.
- Preserve user/project terminology.
- Reference `AGENTS.md` and relevant project docs instead of repeating their contents.
- Include only current Round context/evidence not already available in repo instructions/docs.
- State scope/non-goals only when they are material to the current task.
- Include acceptance criteria and regression guardrails proportionate to risk.
- Tell Codex whether the task is PLAN ONLY, REVIEW ONLY, DIAGNOSE ONLY, or IMPLEMENT.
- Explicitly state `DO NOT COMMIT/TAG/PUSH` until the acceptance gate is reached.
- Explicitly state `DO NOT START NEXT ROUND/PHASE` when applicable.
- Ask Codex to stop after the requested stage.
- After `/plan` PASS in the same session, make `/goal` compact and do **not** repeat the approved plan; carry only review deltas/guardrails and verification/stop rules.
- Ask for concise reports; do not request large diffs/logs or plan restatement when not needed.
- For blocker/STOP/architecture/data-safety/invalidated-assumption reports, require the non-technical summary block from `references/codex-prompts.md` before technical evidence.
- If `docs/CURRENT_PHASE.md` exists, treat it as the compact authority for active Phase/Round state and next action; do not use it as a substitute for detailed specs.
- Never list every PRD/roadmap/UI/checkpoint document in a Codex prompt merely because the Round is high-risk. Start with `AGENTS.md`, `docs/CURRENT_PHASE.md` when present, current worktree evidence, and targeted relevant docs/code.
- Run a prompt preflight before handoff: remove repeated repository-stable rules, repeated approved-plan content, irrelevant docs, oversized verification, and unnecessary scope restatement.
- For UI Rounds, reference the approved Master/UI spec and require no unauthorized visual redesign.

Use templates from `references/codex-prompts.md`; adapt them to the project rather than copying irrelevant sections. For token/credit optimization, read `references/token-efficiency.md`.

## Review rules

### Review a Codex `/plan`

Check:

- root cause vs symptom patch;
- canonical authority/data flow;
- scope creep;
- backward compatibility;
- migration/data-safety implications;
- regression surface;
- tests and manual acceptance;
- UI design fidelity when applicable;
- whether the proposal creates duplicate/parallel sources of truth;
- whether solution complexity has materially exceeded the original problem and should trigger a Complexity Escalation Check.

Return `PASS` or `REVISE` with actionable reasons.

If the plan/report is high-risk or too technical for a non-technical user, explain the practical meaning first; do not jump straight to another Codex prompt.

### Review implementation

Do not infer correctness from Codex's summary alone. Evaluate tests, evidence, screenshots, logs, changed behavior, and manual acceptance needs.

If implementation reports a blocker, STOP, invalid assumption, or new architecture dependency, run the User Understanding Gate before recommending implementation continuation.

### Review a manual-smoke discrepancy

If actual behavior contradicts prior assumptions, first inspect/diagnose. Classify the gap as:

- **INTENTIONAL SCOPE**
- **DOCUMENTED DEFERRED**
- **IMPLEMENTATION GAP**

An implementation gap against an accepted requirement blocks checkpoint readiness.

Use `references/review-checklists.md`.

## Final regression and checkpoint policy

At the end of a Phase, prefer an independent new Codex session, normally Sol High, for REVIEW ONLY; use Extra High only when Phase risk/uncertainty is unusually high.

Review the diff from the previous stable checkpoint to the current working tree. Separate:

- confirmed-correct areas;
- blockers affecting correctness/data safety/stability/accepted behavior;
- non-blocking observations;
- final verification plan;
- readiness verdict.

For each high-risk blocker, explain the practical issue to the user before creating a Fix Round prompt.

If a post-review fix changes source code:

- use **Final Delta Verification** for narrow, low-risk fixes;
- rerun the broader Final Regression Review if the fix touches migrations, canonical data, persistence, worker/concurrency, destructive filesystem behavior, security, or broad architecture.

Only after final manual PASS may the checkpoint prompt authorize:

`tests -> build -> smoke -> docs -> commit -> tag -> push branch -> push tag -> verify remote -> clean working tree`

## Language and output style

Default to Vietnamese unless the user explicitly requests another language. Assume the user may have no software-development background.

- Explain important decisions in plain, practical Vietnamese.
- For complex/high-risk topics, start with the non-technical layer: what happened, a concrete analogy/example, severity, whether damage actually happened, and the recommended next direction.
- Use wording understandable to a typical Grade 10 student; shorter sentences and concrete nouns are preferable to abstract technical jargon.
- Keep precise English technical terms when they are standard or clearer; briefly explain them in Vietnamese on first use when useful.
- Never translate file names, paths, commands, code, class/function/variable names, APIs, endpoints, database fields, model names, identifiers, or error codes.
- Avoid unnecessary jargon and do not translate terminology merely to make prose look more Vietnamese if accuracy suffers.
- Separate the user-facing explanation from optional technical detail when that improves comprehension.
- Be direct and operational: explain why the next step matters, give one recommended next action rather than many competing paths, and keep reports concise.
- Give copyable Codex prompts when the workflow reaches a Codex handoff, but never make the prompt the first thing the user sees when a required User Understanding Gate has not yet been satisfied.
- Do not force the user to remember stage names.

## Resource map

Load only what is needed:

- Full workflow/state machine: `references/workflow.md`
- Non-technical explanation protocol and User Understanding Gate: `references/nontechnical-communication.md`
- Codex prompt templates: `references/codex-prompts.md`
- Dynamic model choice: `references/model-selection.md`
- UI/UX + ui-ux-pro-max integration: `references/ui-ux-integration.md`
- Plan/QA/final-review checklists: `references/review-checklists.md`
- ChatGPT Project usage and chat organization: `references/project-usage.md`
- GitHub stable-vs-working-tree rules: `references/github-usage.md`
- Detailed installation/user guide: `references/user-guide.md`
- Codex token/credit efficiency, AGENTS.md, compact prompts, cache-friendly workflow: `references/token-efficiency.md`
- Current Phase/Round state convention and ChatGPT session ownership: `references/project-usage.md`
- Repository current-state template: `assets/CURRENT_PHASE.template.md`
- Cross-conversation state transport template: `assets/SESSION_HANDOFF.template.md`
- Repository structure policy for new projects, major subsystem additions, or proposed restructures: `references/repository-structure.md`
