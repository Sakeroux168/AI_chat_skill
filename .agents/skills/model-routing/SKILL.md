---
name: model-routing
description: Choose Codex agent (Luna/Terra/Sol) and reasoning effort per task; declare both before starting work.
---

# Codex Model Routing

## Agents and allowed reasoning efforts

| Agent | Allowed reasoning |
|---|---|
| **Luna** | Max only (default) |
| **Terra** | High / XHigh |
| **Sol** | High / XHigh / Max |

## Task → routing recommendations

| Task class | Route |
|---|---|
| Mechanical implementation, well-specified tests, docs | Luna **Max** |
| Ordinary cross-module or exploratory implementation | Terra **High** |
| Complex debugging | Terra **XHigh** |
| Schema / Contract / Architecture / Migration / Concurrency / Lifecycle | Sol **High** |
| Extremely hard blocker / critical final gate | Sol **XHigh** / **Max** |

## Hard rules

1. **Never default everything to Sol.** Escalation is earned by task class,
   not habit. Sol time is expensive and better spent on contract-class work.
2. **Before starting any Codex task, tell the user:**
   `模型：XXX　Reasoning：XXX` — one line, no exceptions. The user may
   override; their instruction wins.
3. When in doubt between two tiers, pick the lower and say why; escalate on
   evidence of difficulty, not anxiety.
4. A project's own routing rules override this file where they conflict.
5. Re-route mid-task if the task turns out to be a different class than
   declared — announce the change the same way.
