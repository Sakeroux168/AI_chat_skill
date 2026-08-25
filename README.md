# AI_chat_skill

Global collaboration skills shared across **all** projects, chats, and agents
(ChatGPT, Codex, Claude, Hermes, …). This is not a project codebase — it holds
no product code; only the working rules for how chats and agents collaborate.

Single entry point: [BOOTSTRAP.md](BOOTSTRAP.md). Skill registry:
[SKILLS.md](SKILLS.md). History: [CHANGELOG.md](CHANGELOG.md).

## How to use in a new chat

ChatGPT does not read GitHub automatically — a fresh session needs one
explicit bootstrap instruction. Paste this as the first message:

```
读取 GitHub 仓库 Sakeroux168/AI_chat_skill 的 BOOTSTRAP.md，并按它加载全局 Skill。
```

That is the most reliable opener for a completely new chat. If the current
chat already knows this repo (or has it locally), the short form also works:

```
加载全局 Skill / 开新项目
```

The session then reads `BOOTSTRAP.md` + `chat-orchestrator/SKILL.md` and
lazy-loads everything else only when routed to.

## Global skills + project skills

Two layers coexist:

- **Project skills/contracts** live in each project's repo (e.g.
  `<project>/.agents/skills/`) and govern that project's specifics — frozen
  schemas, ownership maps, architecture invariants.
- **Global skills** (this repo) govern cross-project working style.

When they touch the same matter, the project layer wins; global skills fill
the gaps. Precedence details are in BOOTSTRAP.md.
