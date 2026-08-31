# AI_chat_skill 使用与维护教程（人读版）

> 本教程给你（仓库主人）看，也可以直接丢给任何 AI 当操作手册用。
> 定位：HANDBOOK.zh-CN.md 是 30 秒速览，**本文件是完整说明书**。
> Skill 本体的真源永远是英文 `.agents/skills/**`；本教程教的是"怎么用这套系统、怎么维护它"。
> 最后更新：2026-08-25（对应 V1.1，10 个 Skill）

---

## 第 1 章 认识这个系统

### 1.1 它是什么 / 不是什么

- **是**：跨所有项目共享的协作规则库。新 ChatGPT 会话、Codex、Claude、Hermes 都从它启动，拿到同一套工作方式。
- **不是**：产品代码库。这里没有任何项目功能代码。

### 1.2 目录导览

```
AI_chat_skill/
├── BOOTSTRAP.md              ← 唯一入口，AI 启动只读这一个 + 路由器
├── README.md                 ← 仓库门面
├── SKILLS.md                 ← Skill 注册表（10 个的一览表）
├── CHANGELOG.md              ← 版本历史（日期版本号 V1.1）
├── HANDBOOK.zh-CN.md         ← 中文速览（30 秒版）
├── TUTORIAL.zh-CN.md         ← 本文件（完整教程）
├── .agents/skills/           ← 10 个 Skill 正文（英文，真源）
│   ├── chat-orchestrator/    ← 路由器（启动必装）
│   ├── requirement-grill/    ← 需求拷问
│   ├── agent-task-dispatch/  ← 任务单格式
│   ├── agent-routing/        ← 角色分工
│   ├── model-routing/        ← Codex 选型 Luna/Terra/Sol
│   ├── engineering-discipline/ ← 干活纪律
│   ├── code-review/          ← 审查规则
│   ├── pr-delivery/          ← 报告写进 PR 的规矩
│   ├── project-hygiene/      ← 什么不许进库
│   └── gui-acceptance/       ← 真人视觉验收规则
└── tests/scenarios/          ← 自测场景（改 Skill 必须过）
```

### 1.3 三条铁律（全系统的根）

1. **Lazy-load**：AI 默认只装 2 个文件，其余按需加载。别让任何改动破坏这条。
2. **优先级**：你当前的话 > 项目自己的 Contract > 本库全局 Skill > 通用默认。
3. **报告进 PR**：Agent 干完活的完整报告必须写进 PR Description/comment，聊天里只回一行确认。

---

## 第 2 章 日常使用（你的部分）

### 2.1 新会话怎么开

**全新会话**（ChatGPT 还不知道这个仓库），第一条消息贴：

```
读取 GitHub 仓库 Sakeroux168/AI_chat_skill 的 BOOTSTRAP.md，并按它加载全局 Skill。
```

**已经知道仓库的会话**（比如有本地 clone），短口令即可：

```
加载全局 Skill
```

就这两句，没有第三种姿势。AI 不会自己发现这个仓库——必须你先给一次入口指令。

### 2.2 启动后会发生什么

你正常说话即可。orchestrator（路由器）拿你的话对比它的路由表：

| 你说的话像什么 | 自动走到哪 |
|---|---|
| "我想做一个…"、"批量…"、"自动…"、"类似某某软件" | requirement-grill |
| "把 tezt 改成 test" 这类一行改动 | 不进流程，直接干 |
| "这活分给几个人/几个 Agent" | agent-routing |
| "交给 Codex 干" | model-routing（+agent-task-dispatch） |
| "帮我审一下这个 PR" | code-review |
| Agent 说"我做完了" | pr-delivery |
| 改了 UI/字体/颜色 | gui-acceptance |
| 你让 AI 写任务单给别人 | agent-task-dispatch |

不需要报 Skill 名字。想强制走某条流程才点名："先 grill 我"、"$requirement-grill"。

### 2.3 六个典型剧本

**剧本 A：模糊的新想法**
你说"我想做个自动整理相册的软件" → grill 开始，一轮 3–6 个问题，每题带推荐答案 → 你答完继续追问，直到产出 Requirement Decision Sheet → 最后一行 `Ready for Planning: YES` 之后才开始设计实现。"你决定"会被记成 DELEGATED 由架构 Agent 定，"不知道"记成 UNKNOWN 不会偷偷替你编。

**剧本 B：小修改**
"README 把 tezt 改成 test" → 没有 grill 没有仪式，直接改。如果它非要问你一堆问题，说一句"这是小修改不用 grill"。

**剧本 C：派 Codex 任务**
开工前它会报 `模型：XXX Reasoning：XXX`（如 Luna Max / Sol High）。你不满意可以直接改它的选择，你的话最大。

**剧本 D：Agent 干完了**
完整报告在 PR 里，聊天里只有一行"已写进 PR #XX，无需向 GPT 复述。"——看到这行就是成功了；看到"未写进 PR，需要向 GPT 转述"才是失败。

**剧本 E：审查**
审查者必须给证据（引用具体行/输出），不能只说"PASS"。它自己说自己做对了不算数。

**剧本 F：视觉改动**
凡是改 UI 的交付，必须出现"真人视觉验收"标记——意思是机器没验证过效果，需要你亲眼看。截图只能当证据，不能当结论。

### 2.4 会话跑偏了怎么办

聊久了模型会漂移（忘了规则、开始瞎猜、把所有 Skill 都塞进上下文）。一句话拉回来：

```
按 BOOTSTRAP 重新路由
```

---

## 第 3 章 维护（加 Skill / 改 Skill）

> 本章可直接丢给 AI 执行。用户说"加一个 skill"，AI 从 3.2 开始照做。

### 3.1 先判断归属（最重要的一步）

```
这条规则只属于某一个项目？
  ├─ 是 → 放那个项目的 .agents/skills/，本库不加
  └─ 否（跨项目通用）→ 继续 3.2
```
拿不准时默认放项目侧——本库每个 Skill 都吃上下文，宁缺毋滥。

### 3.2 加新 Skill 标准流程（SOP）

```
1：准备
从 main 拉分支：feature/<skill-name>

2：建正文
新建 .agents/skills/<skill-name>/SKILL.md，frontmatter 两行必备：
---
name: <skill-name>
description: 一句话说明什么时候用它
---
正文写清：触发条件、行为规则、输出格式。能一页写完就不要写两页。

3：三处登记（缺一处 = 死 Skill，永远不会被触发）
a. SKILLS.md 注册表加一行
b. BOOTSTRAP.md 的 lazy-load 表加一行
c. chat-orchestrator/SKILL.md 路由表加一行：什么信号 → 路由到它

4：自测
tests/scenarios/README.md 至少加 1 个场景：输入什么、期望走什么路径。

5：记录
CHANGELOG.md 顶部加条目，版本号用日期式（如 V1.2 (2026-09-01)），
禁止把分支名写进版本语义。

6：交付
commit → push → Draft PR 到 main → 等 GPT Review → 修复 → merge。
完整过程报告写进 PR Description（pr-delivery 规则）。
```

### 3.3 改现有 Skill

1. 拉 `feature/<skill-name>-update` 分支
2. 只改目标 SKILL.md，不顺手重排别人的段落
3. 受影响的 scenarios 同步更新
4. CHANGELOG 记一条
5. PR 说明"为什么现在的写法不够"

红线：不许把别的 Skill 正文复制进来（引用名字即可）；不许把某个项目的专属规则泛化成"全局"。

### 3.4 删除 / 废弃 Skill

- 从 `.agents/skills/` 删目录 + 三处登记同步删 + CHANGELOG 记录原因
- 已被其他 Skill 引用的，先把引用改成指向替代品

### 3.5 中英双语规则

- **英文 SKILL.md = 唯一真源**，AI 只读英文
- 中文材料（README / HANDBOOK / TUTORIAL）是给人看的导读
- 英文原文变更后，中文导读要跟上；两边冲突时以英文为准
- 新 Skill 用中文写正文可以接受（如 agent-task-dispatch），但 description 里给出英文关键词方便路由匹配

### 3.6 发布约定

- 版本号：日期式 `V<主>.<次> (<YYYY-MM-DD>)`，如 `V1.2 (2026-09-01)`
- 加 Skill = 次 +1；大重构 = 主 +1
- main 必须随时可用：所有变更走分支 + Draft PR，不直接推 main

### 3.7 AI 执行检查清单（加 Skill 时逐项打勾）

```
[ ] 3.1 归属判断已确认是跨项目通用
[ ] SKILL.md 有 name + description frontmatter
[ ] SKILLS.md 已登记
[ ] BOOTSTRAP.md 表已登记
[ ] chat-orchestrator 路由表已登记
[ ] tests/scenarios 有新场景
[ ] CHANGELOG 已记录（日期版本号）
[ ] Draft PR 已建，报告在 PR Description 里
[ ] 未复制其他 Skill 正文
[ ] 未把项目专属规则带进全局
```

---

## 第 4 章 故障排查 FAQ

| 症状 | 原因 | 处理 |
|---|---|---|
| 新会话不知道这些规则 | 没贴启动句 | 贴 2.1 的完整启动句 |
| 该 grill 的没 grill，直接开干了 | 路由漂移 | 说"先 grill 我"强制触发；事后"按 BOOTSTRAP 重新路由" |
| 小修改也被问一堆问题 | grill 误触发 | 说"这是小修改，不用 grill" |
| Codex 开工没报模型和推理档位 | 违反 model-routing 铁律 | 要求补报，并考虑给该 Skill 提修复 PR |
| Agent 把完整报告贴在聊天里 | 违反 pr-delivery | 要求移进 PR，聊天只留一行 |
| 它声称"视觉验证过了" | 违反 gui-acceptance | 不承认该结论，要求标真人验收 |
| 加了新 Skill 但从不触发 | 三处登记漏了一处（多半是路由表） | 按 3.7 清单逐项查 |

---

## 附：一页速查

```text
开新会话     ：贴启动句（2.1）
强制拷问     ：先 grill 我
派 Codex    ：等它报 模型/Reasoning，不满意直接改
看进度      ：去 GitHub 看 PR，聊天里只有一行确认
跑偏急救    ：按 BOOTSTRAP 重新路由
加 Skill    ：第 3.2 节 SOP，AI 可代执行
改 Skill    ：3.3，红线是不复制不夹带
版本        ：日期式，main 随时可用的 Draft PR 流
冲突        ：项目 Contract 永远赢全局
```
