# Dynamic Codex Model & Reasoning Selection

## Table of contents

1. Principle
2. Default decision tree
3. Luna
4. Terra Medium/High
5. Sol High
6. Sol Extra High
7. Reasoning-effort rule
8. Mid-session risk changes
9. Independent reviews
10. UI-specific guidance
11. Token/credit note

## Principle

Choose the **cheapest safe model + reasoning effort for the whole Round**. Do not default to the strongest model merely because the workflow uses `/plan` or `/goal`.

Model choice should follow failure cost, uncertainty, persistence/data risk, concurrency/destructive behavior, and breadth of change.

Keep one model for the active Round session unless the session is explicitly abandoned because new evidence changes the risk class.

## Default decision tree

```text
Tiny + isolated + mechanical + easy to verify?
├─ Yes -> Luna
└─ No
   Normal bounded feature/bug/UI/API work?
   ├─ Yes -> Terra Medium/High
   └─ No
      High-risk architecture/data/recovery/concurrency/security?
      ├─ Yes -> Sol High
      └─ Exceptionally high failure cost/uncertainty?
         └─ Sol Extra High
```

## Luna — lowest-cost default for truly small work

Use when all are true:
- expected behavior is obvious;
- scope is isolated;
- no schema/migration/persistence/recovery/security/destructive behavior;
- few files/components;
- easy focused verification;
- no difficult root-cause analysis.

Examples:
- copy/label change;
- isolated CSS spacing;
- simple icon/tooltip;
- straightforward test fixture cleanup;
- mechanical rename with obvious references.

Do not use Luna just because the visible diff is small if the underlying risk is high.

## Terra Medium/High — normal default

Use for most routine implementation work:
- normal product features;
- bounded bug fixes with plausible root cause;
- UI implementation;
- API wiring;
- local state changes with limited persistence risk;
- routine `/plan` + `/goal` Rounds;
- focused low-risk Fix Rounds.

Prefer **Medium** when the task is well specified and easy to verify.
Use **High** when the task spans several modules, has ambiguity, or needs careful reasoning but is not in the Sol risk class.

Do not use Terra Extra High by default merely to “be safe”; raise the model only when risk/uncertainty justifies it.

## Sol High — high-risk default

Prefer Sol High when one or more are central to the Round:
- architecture redesign;
- schema or migration changes;
- canonical data model changes;
- recovery/restore/rollback;
- reconciliation/repair of persisted data;
- destructive filesystem operations;
- authentication/security;
- worker/job lifecycle or concurrency;
- difficult unknown root cause spanning multiple subsystems;
- broad cross-module change;
- independent review of a high-risk Round;
- final Phase regression review.

## Sol Extra High — exception, not default

Use only when failure cost or uncertainty is unusually high, for example:
- destructive restore/data replacement with weak rollback evidence;
- ambiguous migration/recovery logic where a mistake can lose real user data;
- broad concurrency/recovery design with unresolved invariants;
- Sol High has already failed to resolve a genuinely difficult blocker reliably.

If Sol High is already producing a clear, reviewable plan with adequate tests and safety reasoning, do not upgrade merely for marginal confidence.

## Reasoning-effort rule

Use the lowest effort that safely covers the Round:

- obvious/mechanical -> low/default effort where available;
- normal bounded work -> Medium;
- meaningful ambiguity/cross-module reasoning -> High;
- unusually high-risk/uncertain work -> Extra High only when justified.

Visible answer length is not a proxy for reasoning usage. Avoid asking for long narrative reports when a concise evidence summary is enough.

## Do not switch mid-session casually

If a Terra/Luna `/plan` reveals a migration/data-safety/concurrency/recovery problem:

1. stop before `/goal`;
2. preserve the plan/evidence;
3. open a fresh Sol session;
4. give a compact summary of the new high-risk evidence;
5. re-evaluate there.

Do not silently switch models while pretending session continuity is unchanged.

## Independent reviews

Do not use Sol independent review for every Round.

Prefer it when:
- the Round is high-risk;
- manual QA exposed ambiguous behavior;
- implementation touches migration/persistence/recovery/concurrency/security;
- it is the final Phase regression review.

A small isolated Terra/Luna change can usually proceed with proportionate implementation review + manual QA instead.

## UI-specific guidance

UI work is not automatically Luna.

Use:
- Terra Medium/High for most substantial UI/UX implementation;
- Sol High only when UI work also changes major architecture/state/data behavior or requires an independent high-risk audit;
- Luna for truly small cosmetic fixes.

## Token/credit note

Model, reasoning effort, context size, tools, runtime, and task complexity all affect Codex usage. See `token-efficiency.md` for the full efficiency policy.
