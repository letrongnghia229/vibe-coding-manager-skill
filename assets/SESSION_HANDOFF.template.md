# Session Handoff

Use this when moving active project state into a fresh ChatGPT conversation that may not have direct access to the local repository. Keep it short; it is a transport copy of current state, not a spec or implementation report.

```text
SESSION HANDOFF
Project: <project name>
Phase: <phase number/name>
Previous stable checkpoint: <tag/commit/phase>

Rounds:
- <N.1> <name> — <status>
- <N.2> <name> — <status>

Current: <Round + workflow state>
Working tree: <checkpointed/uncommitted/mixed + whether remote may be older>
Model: <when known>
Known pending evidence: <only material unresolved item, if any>

Next: <exactly one concrete next action>

Do not:
- <critical safety rule>
- <checkpoint/next-phase rule>

Local authority: docs/CURRENT_PHASE.md
```

Rules:
- Generate from the freshest local/Codex evidence available.
- Do not claim ChatGPT read `docs/CURRENT_PHASE.md` unless the current environment actually has access to it.
- Do not paste full plans, PRDs, logs, or conversation history.
- If handoff state conflicts with an older stable GitHub file during uncommitted work, prefer fresh local/Codex evidence and flag the mismatch.
