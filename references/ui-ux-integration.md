# UI/UX Integration with ui-ux-pro-max

## Table of contents

1. Purpose and responsibilities
2. UI Design Gate triggers
3. New project / major UI workflow
4. Master Design System policy
5. Existing-app retrofit path
6. Simple design-system baseline
7. Installing and using ui-ux-pro-max on Codex
8. Whether installation is necessary
9. Whether vibe-coding-manager belongs on Codex
10. UI `/plan` and `/goal` integration
11. Visual QA


## Purpose

Use `ui-ux-pro-max` as a specialist for UI design intelligence. Keep `vibe-coding-manager` as the workflow orchestrator.

Do not merge the third-party skill's full data/scripts into this skill. Keep the dependency separate so it can be updated independently.

Upstream project:

https://github.com/nextlevelbuilder/ui-ux-pro-max-skill

## Division of responsibility

### vibe-coding-manager

Owns:
- when UI design is required;
- approval gates;
- product/UX scope control;
- Round boundaries;
- Codex model/session choice;
- `/plan` and `/goal` workflow;
- functional QA;
- visual QA;
- regression/checkpoint policy.

### ui-ux-pro-max

Use for:
- design-system candidates;
- layout/style direction;
- color/typography/spacing guidance;
- UX/accessibility checks;
- responsive/interaction guidance;
- stack-specific UI implementation advice.

It must not override:
- approved product requirements;
- repository/project rules;
- accepted architecture constraints;
- the approved Master Design System;
- explicit user decisions.

## When to trigger the UI Design Gate

Trigger for:
- new page/screen;
- major layout change;
- navigation/information hierarchy;
- new interaction pattern;
- design-system decisions;
- broad visual modernization;
- user complaint that the app feels like "developer UI";
- accessibility/responsive overhaul.

Skip for:
- backend-only work;
- API/database work;
- nonvisual performance fixes;
- tiny text/icon/spacing corrections unless the user explicitly wants design review.

## Preferred workflow for a new project or major new UI area

```text
Functional requirements
      ↓
UI Design Gate
      ↓
Use ui-ux-pro-max for 1–3 coherent candidate directions
      ↓
ChatGPT critique + product fit check
      ↓
User selects/approves one direction
      ↓
Persist approved Master Design System
      ↓
Freeze page/interaction UI spec
      ↓
Round planning
      ↓
Codex /plan -> review -> /goal
      ↓
Functional QA + Visual QA
```

## Master Design System policy

Prefer a repository source of truth such as:

```text
design-system/<project-slug>/MASTER.md
design-system/<project-slug>/pages/<page-name>.md
```

Rules:
1. Create the Master once per coherent design direction.
2. Review and approve it before broad implementation.
3. Do not regenerate a new style every Round.
4. Page files may override Master only where the page genuinely needs an exception.
5. Existing Master rules beat fresh generic recommendations.
6. Never overwrite an existing approved Master without explicit user authorization.

The upstream ui-ux-pro-max skill supports this Master + page-override pattern.

## Existing app with developer-looking UI: Retrofit path

Do not rewrite the entire app at once.

Use:

```text
UI AUDIT
  ↓
Classify screens/components:
KEEP / POLISH / REDESIGN / INCONSISTENT
  ↓
Decide whether UX is wrong or only visual styling is weak
  ↓
Generate 2–3 retrofit design directions
  ↓
User selects direction
  ↓
Create MASTER.md
  ↓
Migrate tokens/shared components
  ↓
Migrate one screen/flow per Round
  ↓
Functional QA + Visual QA after each Round
  ↓
Final visual regression
```

Recommended modernization order:
1. tokens: color, typography, spacing, radius, borders, shadows;
2. shared components: button, input, select, card, menu, dialog, tabs, tooltip, badge;
3. highest-value screen;
4. remaining screens one at a time;
5. empty/loading/error/responsive states;
6. final visual regression.

Do not mix a broad visual redesign with unrelated database/worker/backend architecture changes in one Round.

## Simple design-system baseline when you want lower complexity

If ui-ux-pro-max is unavailable or unnecessary, use a restrained baseline instead of asking AI to "make it pretty":

- one modern content-first visual direction;
- one primary accent color plus neutral surfaces and semantic status colors;
- one sans-serif family and a small type scale;
- consistent 4px/8px spacing system;
- limited radius set such as 8/12px;
- subtle shadow policy;
- one icon library;
- consistent Button/Card/Input/Menu/Tabs/Dialog primitives;
- subtle motion only, typically 150–250ms;
- no random gradients, arbitrary colors, emoji icons, or per-page style changes;
- explicit focus, contrast, overflow, and responsive rules.

A coherent small system usually produces better results than repeatedly asking Codex to beautify individual screens.

## How to use ui-ux-pro-max on Codex

The upstream project officially supports Codex CLI. Its documented per-project installation flow is:

```bash
npm install -g ui-ux-pro-max-cli
cd /path/to/your/project
uipro init --ai codex
```

Python 3.x is required by its local search scripts. The upstream package states that the scripts use the standard library and do not need external Python packages.

For updates, follow the upstream CLI instructions rather than copying this skill's files into your project manually.

## Is installing ui-ux-pro-max on Codex necessary?

**No, not strictly necessary.**

There are two supported operating modes:

### Mode A — Simple / lowest complexity

- Install `vibe-coding-manager` in ChatGPT Web.
- Do UI Design Gate and approval in ChatGPT.
- Store the approved `MASTER.md` / UI spec in the project repository.
- Codex implements from those approved files without ui-ux-pro-max installed.

Use this when:
- the Master is already clear;
- UI changes are modest;
- you want minimal agent complexity.

### Mode B — Recommended for UI-heavy projects

- Keep `vibe-coding-manager` in ChatGPT Web.
- Install `ui-ux-pro-max` in the Codex project.
- During UI `/plan` and `/goal`, tell Codex to use ui-ux-pro-max only as a specialist and to obey the approved Master/UI spec.

Benefits:
- stack-specific UI guidance is available where implementation happens;
- easier design-system generation/targeted UX searches;
- stronger UI review during UI-heavy Rounds.

Costs:
- one extra local skill/dependency to update;
- more instructions in Codex context;
- without guardrails, a UI specialist may suggest changes beyond the approved scope.

For the user's workflow, prefer **Mode B for projects with substantial UI work**, and Mode A for simple internal tools.

## Should vibe-coding-manager itself be installed on Codex?

**Usually no.**

Keep `vibe-coding-manager` in ChatGPT Web because ChatGPT is the manager/reviewer and Codex is the executor.

Installing the manager skill into Codex can blur responsibilities and create competing workflow control. Codex should receive the current `/plan` or `/goal` prompt plus project docs, not manage the overall approval/checkpoint lifecycle itself.

Exception: if the user later chooses to use Codex as the primary orchestrator instead of ChatGPT Web, create a separate Codex-oriented variant rather than silently reusing the same operating contract.

## How a UI `/plan` should reference ui-ux-pro-max

Add a guardrail like:

```text
UI SPECIALIST:
If ui-ux-pro-max is installed in this repository, use it to check layout, accessibility,
responsive behavior, typography/spacing, and stack-specific UI guidance.

The approved design-system/<project>/MASTER.md and page UI spec are authoritative.
Do not regenerate or replace the Master.
Do not expand product scope based on generic UI recommendations.
```

## How a new Master may be generated with upstream ui-ux-pro-max

The upstream skill documents a design-system search/persistence workflow. A typical pattern is:

```bash
python <ui-ux-pro-max-skill-path>/scripts/search.py \
  "<product type> <industry> <style keywords>" \
  --design-system \
  --persist \
  -p "<Project Name>" \
  --output-dir "<project-root>"
```

Do not run `--force` against an existing Master without explicit user approval.

Because installation paths can differ by environment, let the installed skill/CLI provide the exact local script path rather than hard-coding one in project prompts.

## Visual QA output

For each UI Round, review screenshots against:
- approved Master/page spec;
- hierarchy;
- spacing/alignment;
- typography/readability;
- contrast;
- icon/component consistency;
- scroll/overflow;
- responsive states;
- loading/empty/error states;
- focus/accessibility;
- unintended visual regressions.

Return:
- MUST FIX;
- SHOULD FIX;
- OPTIONAL.

Do not convert OPTIONAL polish into a blocker unless it violates an accepted requirement or accessibility/safety constraint.
