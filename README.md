# AI_chat_skill

这是一个跨所有项目、聊天和 Agent 共享的全局协作 Skill 仓库，适用于 ChatGPT、Codex、Claude、Hermes 等。

它不是项目代码仓库，不保存产品代码，只保存聊天与 Agent 协作时使用的工作规则。

单一入口：[BOOTSTRAP.md](BOOTSTRAP.md)。Skill 注册表：[SKILLS.md](SKILLS.md)。变更历史：[CHANGELOG.md](CHANGELOG.md)。

## 新聊天如何使用

ChatGPT 不会自动读取 GitHub。全新的会话需要先收到一次明确的启动指令。第一条消息直接发送：

```text
读取 GitHub 仓库 Sakeroux168/AI_chat_skill 的 BOOTSTRAP.md，并按它加载全局 Skill。
```

这是新聊天最可靠的启动方式。如果当前聊天已经知道这个仓库，或者本地已经有仓库，也可以使用短指令：

```text
加载全局 Skill / 开新项目
```

会话随后只读取 `BOOTSTRAP.md` 和 `chat-orchestrator/SKILL.md`，其余 Skill 按实际任务需要再懒加载，不会一次性全部塞进上下文。

## 全局 Skill 与项目 Skill

两层规则可以同时存在：

- 项目 Skill / Contract 放在各自项目仓库中，例如 `<project>/.agents/skills/`，负责该项目自己的冻结 Schema、Ownership、架构不变量等规则。
- 全局 Skill 放在本仓库，负责跨项目通用的协作方式、任务派发、模型路由、Review、PR 交付等规则。

当两层规则涉及同一件事时，以项目规则为准；全局 Skill 只补充项目没有规定的部分。完整优先级见 [BOOTSTRAP.md](BOOTSTRAP.md)。
