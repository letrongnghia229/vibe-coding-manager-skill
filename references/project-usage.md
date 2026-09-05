# ChatGPT Project Usage

## Mental model

- **Project = WHAT**
- **Skill = HOW**
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

## Suggested chat organization

```text
Project
├─ 00 - Project Control
├─ Phase N - Main Working Chat
├─ Phase N+1 - Main Working Chat
├─ Independent Review - Phase N
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
- an independent review is desired;
- you want a clean reviewer perspective.

### Codex sessions

Use a stricter boundary:

`1 Round = 1 Codex session = 1 model`

Keep `/plan -> review -> /goal -> implementation -> Round QA` aligned to that Round.

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

The skill should infer the stage and generate the appropriate next action.
