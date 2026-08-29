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
| Clear local/reversible low-risk change (one-line fix, text/style tweak, fully specified local behavior) | no extra skill — proceed under `engineering-discipline` low-risk scope rules |
| Work must be split across agents/roles | `agent-routing` |
| Dispatching a task to Codex | `model-routing` + `agent-task-dispatch` |
| Writing/reformatting a task for Claude, Hermes, Ox, or another agent | `agent-task-dispatch` |
| Formal implementation / debugging / refactor, especially structural or high-risk work | `engineering-discipline` |
| Agent finished GitHub work and must report | `pr-delivery` |
| Reviewing a PR or another agent's output | `code-review` |
| UI / visual / interaction change needs acceptance | `gui-acceptance` |
| Deciding what to commit | `project-hygiene` |
| A reusable professional capability is materially useful but absent from project/global skills | Query `Sakeroux168/AI_shared_skills/registry/skills.json`, then load only the matched skill |

## Routing rules

1. Load at most the routed skills — one primary, plus references it names.
   Never preload the whole registry.
2. A request can chain: grill → route → dispatch → implement → deliver → review.
   Each phase loads its own skill when reached.
3. If the current project defines its own skills or contracts, they take
   precedence over these globals for project-specific matters.
4. If no route fits, proceed with generic care (read before writing, minimal
   change) instead of forcing a skill.
5. When implementation is about to start, classify the **current change** by
   actual risk rather than project size or code volume. Local/reversible work
   stays minimal and focused; structural/high-risk work uses the stronger
   evidence, regression, E2E, human-verification, and authorization boundaries
   defined by `engineering-discipline`.
6. When dispatching implementation work to Codex, load `model-routing` first,
   state Model + Reasoning to the user, then format the task with
   `agent-task-dispatch`.
7. When dispatching work to any other agent, use `agent-task-dispatch` unless
   the user explicitly asks for another format.
8. Discover professional capabilities by matching registry metadata
   (`description`, `category`, and `capabilities`). Do not
   add domain-to-skill mappings here.
9. Load an active `trusted` professional skill by default. Load an
   `experimental` skill only after explicit user request or acceptance;
   never load a `disabled` skill. If the registry cannot be reached,
   state that and proceed with available rules instead of guessing.
