# ChatGPT Project Usage

## Table of contents

1. Mental model
2. What belongs in the ChatGPT Project
3. What belongs in repository `AGENTS.md`
4. Suggested chat organization
5. Codex sessions
6. Context/token-efficient handoff
7. Daily prompt shape

## Mental model

- **Project = WHAT**
- **Skill = HOW**
- **AGENTS.md = stable Codex repository rules**
- **Prompt = NOW**

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

### Phase chat

A practical default is one main ChatGPT chat per Phase. It can cover several Rounds unless context becomes confusing or too large.

Open a fresh ChatGPT chat when:
- the current chat is very long/confused;
- the Round is architecturally distinct;
- an independent reviewer perspective is desired;
- a clean Phase boundary is reached.

Do not open a new ChatGPT chat solely to save Codex context; Codex session boundaries matter more for Codex usage.

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
