# BOOTSTRAP — the single entry point

Once a session is **explicitly bootstrapped** (the user asks it to load the
global skills from this repo — ChatGPT does not read GitHub on its own), it
reads exactly two files by default: this file, plus
`chat-orchestrator/SKILL.md`. Everything else here is **lazy-load**: read a
skill only when the orchestrator routes you to it.

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
| `agent-task-dispatch` | writing or rewriting a task that will be sent to an agent |
| `engineering-discipline` | before implementing/debugging/refactoring |
| `code-review` | reviewing a PR or another agent's completed work |
| `pr-delivery` | an agent finished GitHub work and must report it |
| `project-hygiene` | committing; deciding what belongs in a repo |
| `gui-acceptance` | UI/visual changes need acceptance |

See [SKILLS.md](SKILLS.md) for one-line descriptions.

## How a session gets bootstrapped

A new chat does not read this repo by itself. The user must give an explicit
entry instruction once, e.g.:

> 读取 GitHub 仓库 Sakeroux168/AI_chat_skill 的 BOOTSTRAP.md，并按它加载全局 Skill。

Short forms ("加载全局 Skill") work only when the chat already knows which
repo to load from.
