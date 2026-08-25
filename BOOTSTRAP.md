# BOOTSTRAP — the single entry point

This file is the ONLY thing a new chat / new agent session reads by default
(together with `chat-orchestrator/SKILL.md`). Everything else in this repo is
**lazy-load**: read a skill only when the orchestrator routes you to it.

## What this repository is

Global collaboration skills: working principles that hold for **every**
project, chat, and agent. They are deliberately project-agnostic. Project-
specific rules (contracts, ADRs, frozen schemas, ownership maps) live in each
project's own repo and always take precedence over what is written here.

## Precedence (highest wins)

1. **Current explicit user instruction**
2. **Current project's Contracts / ADRs / project skills**
3. **AI_chat_skill Global Skills (this repo)**
4. Generic defaults

A Global Skill may never override a project's frozen rule. If this repo
conflicts with a project contract, the project contract wins — note the
conflict so it can be fixed here.

## Startup ritual

1. Read `BOOTSTRAP.md` (this file).
2. Read `.agents/skills/chat-orchestrator/SKILL.md`.
3. Do NOT load any other skill until the orchestrator routes to it.
4. If a project defines its own skills, they shadow these globals within
   that project.

## Skill registry (lazy-load only)

| Skill | Load when |
|---|---|
| `chat-orchestrator` | always (at startup) |
| `requirement-grill` | vague new product idea, or user asks to be grilled |
| `agent-routing` | work must be split across agents/roles |
| `model-routing` | dispatching work to Codex (model/reasoning choice) |
| `engineering-discipline` | before implementing/debugging/refactoring |
| `code-review` | reviewing a PR or another agent's completed work |
| `pr-delivery` | an agent finished GitHub work and must report it |
| `project-hygiene` | committing; deciding what belongs in a repo |
| `gui-acceptance` | UI/visual changes need acceptance |

See [SKILLS.md](SKILLS.md) for one-line descriptions.
