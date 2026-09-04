---
name: chat-orchestrator
description: Route an incoming request to the minimum necessary global skill; coordinator owns requirement discovery and task bundling, coding agents do not automatically rerun either.
---

# Chat Orchestrator

The only skill loaded at startup. Its job is routing — nothing else. Load the minimum skill set needed for the current phase.

## Routing table

| Signal in the request | Route to |
|---|---|
| User/coordinator is exploring a genuinely vague product idea or explicitly asks to brainstorm/grill | `requirement-grill` |
| Clear local/reversible change or complete implementation Issue/spec | `engineering-discipline` only; do not rerun requirement discovery |
| Several already-known small/medium issues may share one module/runtime chain or test path | `engineering-discipline` to decide a coherent task bundle; if dispatching afterward, also use `agent-task-dispatch` |
| Work genuinely requires separate agent roles or independent review | `agent-routing` |
| Dispatching a task to Codex | `model-routing` + `agent-task-dispatch` |
| Writing/reformatting a task for Claude, Hermes, Ox, or another agent | `agent-task-dispatch` |
| Formal implementation/debug/refactor | `engineering-discipline` |
| Agent finished GitHub work and must report | `pr-delivery` |
| Reviewing a PR or another agent's output | `code-review` |
| UI/visual/interaction change needs acceptance | `gui-acceptance` |
| Deciding what to commit | `project-hygiene` |
| A reusable professional capability is materially useful but absent from globals/project skills | Query `Sakeroux168/AI_shared_skills/registry/skills.json`, then load only the matched skill |

## Routing rules

1. **Load only what this phase needs.** Never preload the whole registry or run a full skill chain merely because it exists.
2. **A complete spec skips discovery.** If Goal + Scope + Acceptance + Out of Scope are already defined, implementation may proceed directly under engineering discipline. Words such as “自动”, “批量”, “智能”, or “新功能” do not override a complete spec.
3. **Brainstorming belongs to the coordinator/product stage by default.** Coding agents do not independently reopen product discovery unless they find a named blocking business ambiguity.
4. **Task bundling also belongs to the coordinator by default.** Before dispatch, the Chat/coordinator may group several already-known small/medium issues when they share the same module/direct runtime chain, implementation context, and verification path. The implementation agent should receive the prepared bundle rather than spend quota scanning the backlog or repository for more candidates.
5. **Bundle by engineering coherence, not by count.** “One task” may contain multiple known same-chain bugs, but unrelated architecture/schema/performance/product decisions remain separate. Newly discovered adjacent independent work is reported back instead of silently absorbed.
6. **Skill chaining is conditional, not ceremonial.** `grill → bundle/dispatch → implement → deliver → review` is possible, but phases are skipped when unnecessary. Do not mechanically force Architecture/Plan/subagent phases.
7. Project-specific skills/contracts take precedence over global guidance.
8. Before implementation, classify the **current change or task bundle** by actual risk rather than repository importance, code volume, prompt length, or number of bundled micro-fixes.
9. When dispatching Codex, load `model-routing`, state Model + Reasoning, then use `agent-task-dispatch`. Prefer the lower reasonable effort and escalate on evidence.
10. Agent fan-out is not a default route. Use `agent-routing` only when separate ownership/parallelism/independent review is actually needed. A coherent bundle normally keeps one implementation owner for the shared core path; bounded independent QA may be parallelized when the benefit justifies duplicated context/token cost.
11. Discover professional capabilities by registry metadata; do not hard-code domain-specific skills here.
12. Load active `trusted` professional skills by default. `experimental` requires explicit user request/acceptance; never load `disabled`.
13. If no route fits, proceed with generic care: inspect, make the minimum change, test the direct risk, stop when done.
