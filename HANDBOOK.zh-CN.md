# 全局 Skill 中文手册

> 本手册只是**人看的中文导读**，方便你快速了解每个 Skill 是干什么的。
> **AI 一律读英文原文**（`.agents/skills/<name>/SKILL.md`），那份才是真源。
> 两边如有出入，以英文原文为准，并请提 PR 修正本手册。

## 怎么用（30 秒版）

新会话第一条消息贴：

```
读取 GitHub 仓库 Sakeroux168/AI_chat_skill 的 BOOTSTRAP.md，并按它加载全局 Skill。
```

之后 AI 只装两个文件：BOOTSTRAP.md + chat-orchestrator（路由器）。
其余 Skill 按需加载——路由器听你说话，命中才去读，不命中不浪费上下文。

## 优先级（谁说了算）

```
你当前的明确指示
> 项目自己的 Contract / ADR / 项目级 Skill
> 本仓库的全局 Skill
> 通用默认
```

## 10 个 Skill 速览

| Skill | 干什么用 | 什么时候会被加载 |
|---|---|---|
| `chat-orchestrator` | 路由器：判断你的话该走哪个 Skill | 启动时必装 |
| `requirement-grill` | 需求拷问：模糊想法 → 一轮轮追问 → 需求决策表 | "我想做个…"、"批量…"、"先 grill 我" |
| `agent-task-dispatch` | 任务派发格式：低 token、编号式任务单 | 你让 AI 给别的 Agent 写任务时 |
| `agent-routing` | 角色分工：PM/实现/UI/后端/研究/红队/终审 | 活要分给多个 Agent 时 |
| `model-routing` | Codex 选型：Luna/Terra/Sol + 推理档位，开工前必须报 `模型：XXX Reasoning：XXX` | 派 Codex 任务时 |
| `engineering-discipline` | 干活纪律：先查再猜、最小改动、测真实路径、不许为绿测试放水 | 正式写代码/调试前 |
| `code-review` | 审查：P0 数据损坏 > P1 正确性 > P2 兼容 > P3 可维护 > 样式；只信证据不信"我说 PASS" | 审 PR 或别人的产出时 |
| `pr-delivery` | 交差规则：完整报告写进 PR，聊天里只回"已写进 PR #XX，无需向 GPT 复述。" | Agent 完成 GitHub 工作时 |
| `project-hygiene` | 仓库卫生：node_modules/密钥/聊天记录不许进库；长期知识进代码/文档/ADR/Skill | 提交、整理仓库时 |
| `gui-acceptance` | 视觉验收：改 UI/字体/颜色/动画 → 必须标"真人视觉验收"；纯后端不用 | 涉及看得见的变化时 |

## 常用口令

| 你说 | 效果 |
|---|---|
| "先 grill 我" / "$requirement-grill" | 强制进需求拷问 |
| "加载全局 Skill" | 短口令启动（前提：会话已知这个仓库） |
| "按 BOOTSTRAP 重新路由" | 会话漂移了，拉回正轨 |

## 加新 Skill（简版）

1. 跨项目通用才进本库；项目专属放项目自己仓库
2. 建 `.agents/skills/名字/SKILL.md`
3. 三处登记缺一不可：SKILLS.md + BOOTSTRAP.md 表 + chat-orchestrator 路由表
4. tests/scenarios 加至少一个场景 → CHANGELOG 记一笔 → Draft PR

原则：能不加就不加，每个 Skill 都吃上下文；新 Skill 一律 lazy-load。

---

English is the source of truth for agents: see [SKILLS.md](SKILLS.md) and `.agents/skills/**`.
