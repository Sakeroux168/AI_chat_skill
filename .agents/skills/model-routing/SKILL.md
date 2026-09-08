---
name: model-routing
description: Choose an exposed Codex model and reasoning effort by the current task and demonstrated difficulty; declare the actual selection.
---

# Codex Model Routing

Model capability and reasoning effort are separate choices. Route the current
change, not the repository's importance or the prompt's length.

## Availability and starting points

Use only models and reasoning settings exposed by the active harness. Model
names below are conditional candidates, not permanent capability bindings.
Recheck availability when dispatching; never infer GPT-6 Astra support from
this document. Keep cheaper/current routes when they can reliably do the work.

| Task | Candidate model when exposed | Starting effort |
|---|---|---|
| Small/mechanical edit, docs, deterministic formatting | Luna or another capable lower-cost model | Low |
| Ordinary, clearly scoped implementation | Terra or another capable current model | Medium |
| Difficult scoped debugging | Sol; Astra when its extra capability is useful | Medium |
| Long-horizon/complex engineering or structural/high-risk work | Astra when useful; otherwise an exposed capable model such as Sol | Medium |

If Astra is selected for small/mechanical work, start at **Low**; ordinary
implementation and long-chain/complex engineering start at **Medium**.
High risk requires appropriate engineering checks, not automatic High reasoning.
If a recommended model/effort is unavailable, select a supported capable
alternative; report a capability gap only if it actually blocks the task.

## Escalation and declaration

- Raise to **High** only for a demonstrated blocker, complex unresolved root
  cause, or architectural impasse. Reassess after a focused attempt rather
  than mechanically replacing “Sol High” with “Astra High”.
- **XHigh / Max** are reserved for proven extreme difficulty or a critical
  final gate with a concrete need for extra reasoning. Max is opt-in
  quality-first mode; neither is a routine default.
- Before work/dispatch, state `模型：XXX　Reasoning：XXX` using the actual supported
  selection. A recommendation is not proof that the harness switched models;
  if the active effort is not exposed, state UNKNOWN. Announce evidence-based
  changes when rerouting.
- Multi-agent/ultra execution is a separate cost decision, not an automatic
  reasoning upgrade; use `agent-routing` when independent work is warranted.

Explicit user instructions and project routing rules take precedence.
