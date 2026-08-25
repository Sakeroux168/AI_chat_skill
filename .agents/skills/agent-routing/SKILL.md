---
name: agent-routing
description: Assign work to agent roles; roles are stable, model bindings are defaults that projects may override.
---

# Agent Routing

Define **roles**, not permanent model bindings. A role describes what kind of
work an agent does; the mapping of role → actual model/agent is a default
that any project's own ownership rules override.

## Roles

| Role | Scope |
|---|---|
| Product / Architecture Orchestrator | requirement grilling, decomposition, work assignment, final coherence |
| Implementation Agent | writing code/tests/docs to spec in a defined scope |
| UI / Browser Agent | editor/UI/browser-driven work, visual prototyping, DOM-level verification |
| System / Backend Agent | services, Python/systems code, desktop shell, integration plumbing |
| Research Agent | web/codebase investigation, comparisons, cited digests |
| Independent Reviewer / Red Team | adversarial review of others' output; investigate & report only, no cross-ownership edits |
| Final Architecture Gate | freeze/approve contracts, schemas, migrations, irreversible architecture decisions |

## Current default mapping (informational)

| Agent/harness | Default candidate roles |
|---|---|
| ChatGPT (GPT) | PM / Grill / Architecture / Final Gate |
| Claude | UI / Browser / Editor-class implementation |
| Codex | System / Backend / Python / Desktop / Integration |
| Hermes / Ox Alpha | Research / Red Team / Independent Review |

## Rules

1. Project ownership > this default mapping. If a project assigns a path or
   decision to a specific agent, that assignment wins.
2. One agent may hold several roles on small tasks; for contract/schema/
   architecture freezes, the reviewer and author must be different agents.
3. When dispatching to Codex, `model-routing` decides model + reasoning
   effort — do not hand-pick per task without going through it.
4. Red-team/reviewer agents report findings with evidence and do not edit
   the artifacts they review.
5. State Model + Reasoning (and role) when assigning work so the user can
   veto before the task starts.
