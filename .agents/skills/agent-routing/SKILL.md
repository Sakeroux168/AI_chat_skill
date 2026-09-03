---
name: agent-routing
description: Assign work to agent roles only when separation adds real value; default to one implementation agent for bounded tasks.
---

# Agent Routing

Define roles, not permanent model bindings. Use multiple agents only when the task has genuinely independent workstreams or requires independent review.

## Roles

| Role | Scope |
|---|---|
| Product / Architecture Orchestrator | requirement discovery, decomposition, business/architecture decisions, final coherence |
| Implementation Agent | code/tests/docs to an already defined scope |
| UI / Browser Agent | editor/UI/browser-driven implementation and DOM verification |
| System / Backend Agent | services, Python/systems code, desktop shell, integration plumbing |
| Research Agent | explicit research/investigation tasks with bounded questions |
| Independent Reviewer / Red Team | adversarial review of another agent's completed output; report findings, do not silently rewrite it |
| Final Architecture Gate | independent contract/schema/migration/irreversible-architecture review where separation is warranted |

## Current default mapping (informational)

| Agent/harness | Default candidate roles |
|---|---|
| ChatGPT (GPT) | Product / requirement / architecture / final coordination |
| Claude | UI / Browser / Editor-class implementation |
| Codex | Implementation / System / Backend / Desktop / Integration |
| Hermes / Ox Alpha | Research / Red Team / Independent Review |

## Rules

1. **One implementation agent is the default.** Small and moderate tasks should not be split merely to look rigorous.
2. Add another agent only when there is a concrete independent workstream, an ownership boundary, or a real independence requirement such as author vs final contract reviewer.
3. Do not create researcher + implementer + tester + reviewer chains for routine work. Testing belongs to the implementation task unless independent verification materially improves confidence.
4. **No recursive fan-out.** A delegated agent must not spawn further agents just for thoroughness unless the coordinator explicitly authorized that structure.
5. Product brainstorming/requirement discovery remains with the coordinator by default; implementation agents receive a defined scope and do not reopen it unless blocked by an unresolved business decision.
6. For contract/schema/architecture freezes or irreversible decisions, author and final independent reviewer should be different when practical.
7. When dispatching to Codex, `model-routing` decides model + reasoning effort. Do not equate more agents with more reasoning quality.
8. Project ownership rules override this default mapping.
9. State Model + Reasoning and role when assigning work so the user can veto before execution.
