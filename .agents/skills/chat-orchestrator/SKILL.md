---
name: chat-orchestrator
description: Route an incoming request to the minimum necessary global skill; coordinator owns requirement discovery, project continuity, and task bundling, while coding agents do not automatically rerun them.
---

# Chat Orchestrator

The only skill loaded at startup. Its job is routing — nothing else. Load the minimum skill set needed for the current phase.

**Coordinator restraint is part of routing.** These rules constrain Chat/coordinator as well as downstream agents. Do not turn a directly solvable request into extra Issues, plans, research, agents, test matrices, abstractions, or task splits merely because those mechanisms are available. Choose the minimum sufficient work that satisfies the user's current goal; escalate process, risk, or engineering effort only when current evidence requires it.

## Routing table

| Signal in the request | Route to |
|---|---|
| Continuing a project across coordinator sessions, returning after a gap, or preparing project dispatch where coordinator memory exists | `coordinator-continuity` |
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
2. **Project continuity is project-scoped.** When `coordinator-continuity` applies, load the project's current coordinator memory/handoff from that project repository. Do not copy project memory into this global repository and do not reread unchanged memory every few messages.
3. **A complete spec skips discovery.** If Goal + Scope + Acceptance + Out of Scope are already defined, implementation may proceed directly under engineering discipline. Words such as “自动”, “批量”, “智能”, or “新功能” do not override a complete spec.
4. **Brainstorming belongs to the coordinator/product stage by default.** Coding agents do not independently reopen product discovery unless they find a named blocking business ambiguity.
5. **Task bundling also belongs to the coordinator by default.** Before dispatch, the Chat/coordinator may group several already-known small/medium issues when they share the same module/direct runtime chain, implementation context, and verification path. The implementation agent should receive the prepared bundle rather than spend quota scanning the backlog or repository for more candidates.
6. **Bundle by engineering coherence, not by count.** “One task” may contain multiple known same-chain bugs, but unrelated architecture/schema/performance/product decisions remain separate. Newly discovered adjacent independent work is reported back instead of silently absorbed.
7. **Skill chaining is conditional, not ceremonial.** `grill → bundle/dispatch → implement → deliver → review` is possible, but phases are skipped when unnecessary. Do not mechanically force Architecture/Plan/subagent phases.
8. Project-specific skills/contracts take precedence over global guidance.
9. Before implementation, classify the **current change or task bundle** by actual risk rather than repository importance, code volume, prompt length, or number of bundled micro-fixes.
10. When dispatching Codex, load `model-routing`, state Model + Reasoning, then use `agent-task-dispatch`. Prefer the lower reasonable effort and escalate on evidence.
11. Agent fan-out is not a default route. Use `agent-routing` only when separate ownership/parallelism/independent review is actually needed. A coherent bundle normally keeps one implementation owner for the shared core path; bounded independent QA may be parallelized when the benefit justifies duplicated context/token cost.
12. Discover professional capabilities by registry metadata; do not hard-code domain-specific skills here.
13. Load active `trusted` professional skills by default. `experimental` requires explicit user request/acceptance; never load `disabled`.
14. If no route fits, proceed with generic care: inspect, make the minimum change, test the direct risk, stop when done.
