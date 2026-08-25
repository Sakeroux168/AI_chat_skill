---
name: chat-orchestrator
description: Route an incoming request to the right global skill; owns no other content.
---

# Chat Orchestrator

The only skill loaded at startup. Its job is routing — nothing else. It never
copies or summarizes other skills' bodies; it points to them and gets out of
the way.

## Routing table

| Signal in the request | Route to |
|---|---|
| Vague new product idea ("我想做一个…", "自动…", "批量…", "类似某某软件…") or explicit "先 grill 我" | `requirement-grill` |
| Clear small change (one-line fix, text tweak, already fully specified) | no skill — just do it under `engineering-discipline` basics |
| Work must be split across agents/roles | `agent-routing` |
| Dispatching a task to Codex | `model-routing` |
| Formal implementation / debugging / refactor about to start | `engineering-discipline` |
| Agent finished GitHub work and must report | `pr-delivery` |
| Reviewing a PR or another agent's output | `code-review` |
| UI / visual / interaction change needs acceptance | `gui-acceptance` |
| Deciding what to commit | `project-hygiene` |

## Routing rules

1. Load at most the routed skills — one primary, plus references it names.
   Never preload the whole registry.
2. A request can chain: grill → route → implement → deliver → review. Each
   phase loads its own skill when reached.
3. If the current project defines its own skills or contracts, they take
   precedence over these globals for project-specific matters.
4. If no route fits, proceed with generic care (read before writing, minimal
   change) instead of forcing a skill.
5. When routing implementation work to Codex, always pass through
   `model-routing` first and state Model + Reasoning to the user.
