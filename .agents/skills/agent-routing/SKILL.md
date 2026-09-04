---
name: agent-routing
description: Assign work to agent roles only when separation adds real value; coordinator owns task bundling and bounded tasks default to one implementation agent.
---

# Agent Routing

Define roles, not permanent model bindings. Use multiple agents only when the task has genuinely independent workstreams or requires independent review.

## Roles

| Role | Scope |
|---|---|
| Product / Architecture Orchestrator | requirement discovery, decomposition, **task bundling**, business/architecture decisions, final coherence |
| Implementation Agent | code/tests/docs for an already defined single task or coordinator-defined coherent task bundle |
| UI / Browser Agent | editor/UI/browser-driven implementation and DOM verification |
| System / Backend Agent | services, Python/systems code, desktop shell, integration plumbing |
| Research Agent | explicit research/investigation tasks with bounded questions |
| Independent Reviewer / Red Team | adversarial review of another agent's completed output; report findings, do not silently rewrite it |
| Final Architecture Gate | independent contract/schema/migration/irreversible-architecture review where separation is warranted |

## Current default mapping (informational)

| Agent/harness | Default candidate roles |
|---|---|
| ChatGPT (GPT) | Product / requirement / architecture / task bundling / final coordination |
| Claude | UI / Browser / Editor-class implementation |
| Codex | Implementation / System / Backend / Desktop / Integration |
| Hermes / Ox Alpha | Research / Red Team / Independent Review |

## Rules

1. **Task bundling belongs to the coordinator by default.** The coordinator groups already-known same-chain small/medium work before dispatch. Implementation agents should not spend quota scanning the backlog or repository to decide what else should be bundled.
2. **One implementation owner is the default for a coherent bundle.** Small and moderate work that shares one core code/runtime path should not be split across several implementation agents merely because parallelism is available.
3. Add another implementation agent only when there is a concrete independent workstream with low edit overlap, an ownership boundary, or another measurable parallel benefit that exceeds duplicated context/coordination cost.
4. Independent test/QA agents may run in parallel after or beside a shared implementation when their verification paths are genuinely independent and this materially reduces wall-clock time. They should not silently reimplement or rewrite the same core path.
5. Do not create researcher + implementer + tester + reviewer chains for routine work. Testing normally belongs to the implementation task unless independent verification materially improves confidence or saves meaningful elapsed time.
6. **No recursive fan-out.** A delegated agent must not spawn further agents just for thoroughness unless the coordinator explicitly authorized that structure.
7. Product brainstorming/requirement discovery remains with the coordinator by default; implementation agents receive a defined scope/task bundle and do not reopen it unless blocked by an unresolved business decision.
8. During implementation, a directly shared root cause, acceptance blocker, or regression caused by the current change may stay in the same bundle. Newly discovered adjacent but independent work is reported back to the coordinator instead of silently absorbed.
9. For contract/schema/architecture freezes or irreversible decisions, author and final independent reviewer should be different when practical.
10. When dispatching to Codex, `model-routing` decides model + reasoning effort. Do not equate more agents, more bundled items, or more parallelism with more reasoning quality.
11. Project ownership rules override this default mapping.
12. State Model + Reasoning and role when assigning work so the user can veto before execution.
