# ChatGPT Project Usage

## Table of contents

1. Mental model
2. What belongs in the ChatGPT Project
3. What belongs in repository `AGENTS.md`
4. Suggested chat organization
5. Codex sessions
6. Context/token-efficient handoff
7. Daily prompt shape
8. Current Phase state file
9. Session handoff

## Mental model

- **Project = WHAT**
- **Skill = HOW**
- **AGENTS.md = stable Codex repository rules**
- **docs/CURRENT_PHASE.md = WHERE NOW in the local repository (active Phase/Round state, when used)**
- **Session Handoff = transport of WHERE NOW into a new ChatGPT conversation when local repo access is unavailable**
- **Prompt = NOW / immediate delta**

## What belongs in the ChatGPT Project

Keep project-specific truth here:
- PRD;
- architecture/tech stack;
- roadmap;
- UI/UX specification;
- repository/Codex rules;
- checkpoint history;
- product-specific constraints;
- stable GitHub repository identity.

Do not copy generic vibe-coding workflow rules into every project if this skill already provides them.

## What belongs in repository `AGENTS.md`

Keep it short and durable:
- preferred response language/technical level;
- source-of-truth doc paths;
- stable platform/data/source-write constraints;
- Git safety rules;
- stable build/test entry points;
- compact-report/token-efficiency rules.

For non-technical users, also keep a compact reporting rule for blockers/STOP/high-risk findings: explain the practical problem, consequence, confirmed damage vs risk, and required decision before technical evidence.

Do **not** put active Round status, temporary bugs, long PRD text, or current uncommitted implementation summaries into `AGENTS.md`.

Codex should read detailed project docs only when relevant instead of receiving them pasted into every prompt.

Do not turn `AGENTS.md` into a repository manual. When the user is creating a project, adding a major subsystem, or considering a structural refactor, use `repository-structure.md`; routine Rounds should not load that reference.

## Suggested chat organization

```text
Project
├─ 00 - Project Control
├─ Phase N - Main Working Chat
├─ Phase N+1 - Main Working Chat
├─ Independent Review - Phase N (when needed)
└─ Special Debug / Architecture (only when needed)
```

### Project Control

Use for:
- roadmap decisions;
- architecture/product prioritization;
- future ideas;
- cross-Phase decisions.

### Round chat

Default to one active Round per ChatGPT conversation. Keep brainstorm, critique, freeze, Codex prompt generation, plan review, implementation review, and manual QA for that Round together.

Open a fresh ChatGPT chat when:
- starting a new Round;
- starting a new Phase;
- the current chat is very long/confused;
- an independent reviewer perspective is desired.

This is a reliability default, not a hard blocker: the user may continue in the same conversation when convenient. Do not make the user reconstruct context manually; provide a compact session handoff at the boundary.

### Codex sessions

Use a stricter boundary:

`1 Round = 1 Codex session = 1 model`

Default Round flow:

`/plan -> review -> /goal -> implementation -> focused verification -> report -> manual QA`

Keep `/plan` and `/goal` in the same Codex session so `/goal` can say “implement the approved plan from this session” instead of repeating the plan.

Open a fresh Codex session for:
- a new Round;
- intentional independent review;
- a materially higher risk class/model;
- a confused/compacted session that no longer has reliable context.

## Context/token-efficient handoff

Prefer:

```text
AGENTS.md + repo docs
        ↓
short /plan with current goal/evidence
        ↓
approved plan stays in same session
        ↓
short /goal with review deltas only
```

Avoid:
- pasting the entire PRD/roadmap into `/plan`;
- pasting the approved `/plan` back into `/goal`;
- opening new sessions between plan and implementation;
- giant “context dump” summaries when Codex can read the repo.

See `token-efficiency.md`.

## Daily prompt shape

The user only needs to provide:

```text
CONTEXT
Where am I now?

PROBLEM / GOAL
What should change?

EVIDENCE
Screenshots, logs, Codex output, plan, diff, etc.

REQUEST
What do I need ChatGPT to do next?
```

The skill should infer the stage and generate the smallest sufficient next action.

For complex/high-risk Codex output, the user does not need to ask for a translation. The skill should automatically run the User Understanding Gate before another implementation handoff.

## 8. Current Phase state file

For projects with multiple Rounds, prefer `docs/CURRENT_PHASE.md` as a short mutable state index.

It should contain:
- current Phase;
- previous stable checkpoint;
- Round ledger (`PLANNED`, `FROZEN`, `READY_FOR_PLAN`, `PLAN_PASS`, `IMPLEMENTED`, `ROUND_ACCEPTED`, or an equivalent small vocabulary);
- current Round and state;
- selected model when known;
- whether current work is uncommitted/uncheckpointed;
- exactly one next action.

It should not contain full requirements, long implementation reports, or checkpoint history. Those remain in project specs/roadmap and `CHECKPOINTS.md`.

Use `assets/CURRENT_PHASE.template.md` as the starting structure.

## 9. Session handoff

`docs/CURRENT_PHASE.md` does not magically synchronize into ChatGPT. Distinguish access explicitly:

- Codex working in the local repo can read local `docs/CURRENT_PHASE.md` directly.
- ChatGPT may use it directly only when the file is uploaded, available through an accessible connector/runtime, or otherwise actually readable in the current conversation.
- A new ChatGPT conversation without local-repo access should receive a Session Handoff. Never claim that ChatGPT read the local file when it did not.
- Whenever recommending a fresh ChatGPT or Codex session, include a compact handoff so a non-technical user does not need to remember project state. The handoff should normally be generated from the freshest available local/Codex evidence and should mirror, not replace, `CURRENT_PHASE.md`.

Template:

```text
SESSION HANDOFF
Project: <name>
Phase: <phase>
Previous stable checkpoint: <checkpoint>
Rounds: <compact status list>
Current: <round + workflow state>
Working tree: <checkpointed/uncommitted fact>
Model: <when known>
Known pending evidence: <only material unresolved item, if any>
Next: <exactly one action>
Do not: <critical safety/next-phase rules>
Local authority: docs/CURRENT_PHASE.md
b```

Use `assets/SESSION_HANDOFF.template.md` when a copyable artifact is useful. Do not paste old conversation transcripts or full specs into the handoff.
