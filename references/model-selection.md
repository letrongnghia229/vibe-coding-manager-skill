# Dynamic Codex Model Selection

## Principle

Model choice is dynamic by Round risk, not fixed by workflow stage. Do not use one model for `/plan` and another for `/goal` within the same active Round session.

## Default decision tree

```text
Is this a high-risk / high-uncertainty Round?
├─ Yes -> Sol High or Extra High
└─ No
   Is it tiny, isolated, mechanical, and easy to verify?
   ├─ Yes -> Luna
   └─ No  -> Terra Extra High
```

## Terra Extra High — default

Use for most implementation work:
- normal features;
- bug fixes;
- UI implementation;
- API wiring;
- state-management changes with bounded scope;
- typical `/plan` + `/goal` Rounds;
- focused final Fix Rounds.

## Sol High / Extra High — high risk or independent review

Prefer Sol when one or more of these are central to the Round:
- architecture redesign;
- schema or migration changes;
- canonical data model changes;
- reconciliation/repair of persisted data;
- destructive filesystem operations;
- rollback/recovery guarantees;
- authentication/security;
- worker/job lifecycle or concurrency;
- difficult unknown root cause spanning multiple subsystems;
- broad cross-module change;
- independent final Phase regression review.

Use Extra High when failure cost or uncertainty is unusually high.

## Luna — small isolated work only

Use Luna only when all are true:
- expected behavior is obvious;
- scope is isolated;
- no schema/API/persistence/security/destructive behavior;
- few files/components;
- easy automated/manual regression verification;
- no difficult root-cause analysis.

Typical examples:
- label/copy edit;
- simple tooltip;
- isolated icon replacement;
- small CSS spacing correction;
- straightforward helper test.

## Do not switch mid-session casually

If a Terra `/plan` reveals that the task is actually a migration/data-safety/concurrency problem:

1. stop before `/goal`;
2. preserve the plan/evidence;
3. open a fresh Sol session;
4. re-evaluate with the higher-risk model.

Do not silently switch models while pretending session continuity is unchanged.

## UI-specific guidance

UI work is not automatically Luna.

Use:
- Terra Extra High for most substantial UI/UX implementation;
- Sol only if the UI Round also changes major architecture/state/data behavior or requires an independent audit;
- Luna for truly small cosmetic fixes.
