---
name: model-routing
description: Choose Codex model tier and reasoning effort by actual task risk and measured difficulty; declare both before starting work.
---

# Codex Model Routing

Choose the **current task**, not the importance of the whole repository. Model tier and reasoning effort are separate decisions.

## Available GPT-5.6 reasoning efforts

When the harness exposes the current GPT-5.6 family controls, Luna, Terra, and Sol support `none`, `low`, `medium`, `high`, `xhigh`, and `max`. A harness may expose only a subset; use only settings it actually supports. A user's explicit override wins.

Default principle: **start at the lowest tier/effort that should reliably complete the scoped task, then escalate on evidence.** `medium` is the normal balanced starting point for implementation work. Higher effort is not a quality ritual.

## Task → routing recommendations

| Task class | Default route |
|---|---|
| Mechanical edit, docs, deterministic formatting, well-specified tiny change | Luna **Low/Medium** |
| Ordinary bounded implementation, focused tests, routine cross-module work | Terra **Medium** |
| Exploratory but bounded implementation or unfamiliar codebase | Terra **Medium/High** |
| Difficult but scoped debugging where flagship capability is useful | Sol **Medium** first; raise to **High** only if focused investigation justifies it |
| Schema / Contract / Architecture / Migration / Concurrency / process lifecycle / irreversible high-risk work | Sol **High** |
| Proven hard blocker or critical final gate where extra reasoning has measured value | Sol **XHigh**; **Max** only when quality-first cost is explicitly justified |

## Hard rules

1. **Never default everything to Sol, and never default Sol to High.** Escalation is earned by the current change's risk or demonstrated difficulty.
2. **Before starting any Codex task, tell the user:** `模型：XXX　Reasoning：XXX`. The user may override.
3. **When in doubt, choose the lower reasonable setting.** Escalate after evidence such as a failed focused attempt, unresolved root cause, contract ambiguity, or a genuinely wider blast radius.
4. **"Important product" is not a routing class.** A critical product can contain a one-line reversible edit; route the edit, not the product's business importance.
5. **Prompt length is not task difficulty.** A long task brief does not by itself justify High/XHigh/Max.
6. **Max is opt-in quality-first mode.** Do not use it for routine implementation, mechanical work, ordinary tests, or because more reasoning feels safer.
7. If a harness offers multi-agent/ultra execution, treat it as a separate high-cost capability. Do not enable it by default; require a task that truly benefits from independent parallel work or explicit user approval.
8. Re-route mid-task only when evidence shows the task class changed. Announce the new Model + Reasoning before continuing.
9. A project's own routing rules override this file where they conflict.
