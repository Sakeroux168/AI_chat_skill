---
name: agent-task-dispatch
description: 派发 Codex、Claude、Hermes、Ox 等 Agent 任务时，用低 token、高可读性、边界清楚的工程任务包格式编写任务单。
---

# Agent 任务派发规范

用途：把已经明确的任务整理成 Agent 可直接执行的任务单，同时控制上下文、推理和执行范围。它只规定派发表达与任务大小，不替代项目 Contract/ADR、`model-routing`、`engineering-discipline` 或真正需要的 requirement discovery。

1：一个任务一个清晰工程结果
默认每份任务只包含一个清晰、可验收的工程结果。这个结果可以是一个单独 Bug，也可以是 coordinator 已经打包好的同链路任务包。不要机械执行“一个 Bug = 一个 Agent = 一个 PR”；也不要把“调查 + 重新设计架构 + 实现 + 性能优化 + 全量回归 + 文档宇宙”塞成一个巨型任务。

2：任务打包由 coordinator 默认负责
Chat / Product / Architecture Orchestrator 在派发前负责判断哪些已知问题适合打包。Implementation Agent 不应为了寻找“还能顺便做什么”去扫描整个 backlog、全部 Issue 或整个仓库。
适合打包的条件通常是：同一模块或直接调用链、共享大部分代码上下文、共享大部分测试/运行路径、问题已经确认存在、每个问题都能保留清晰验收。
如果几个任务只是文件位置接近，但需要不同产品决策、Schema/迁移、架构边界、性能专项或不同风险等级，则不要硬打包。

3：完整规格不重复 brainstorm
如果 Issue/任务包已经包含 Goal、Scope、Acceptance、Out of Scope，就把它当实现输入。不要再要求 Coding Agent 重新头脑风暴、重新采访用户、重新写一份同义 spec 或强制走 Architecture → Plan 仪式。只有一个真正阻塞实现的业务决策时，只问/报告那个决策。

4：控制调查边界
默认要求 Agent 先读：任务/Issue、目标实现、直接调用者/消费者、直接相关测试。任务包有多个同链路问题时，共享调查一次即可；不要为每个微小问题从头重复加载同一上下文。只有出现具体证据时才向外扩。不要用“全面调查整个仓库”“读所有相关文件”“穷举所有风险”这类无边界措辞，除非任务本身就是研究/审计。

5：模型与 Reasoning
派发 Codex 前加载 `model-routing`。在任务最前面写 `模型：XXX　Reasoning：XXX`。Medium 是普通实现的常见起点；High/XHigh/Max 必须由当前任务风险或已观察到的困难支持。不要因为项目重要、Prompt 很长、任务包里有多个小问题或“更保险”就升档。

6：Subagent 默认关闭
不要在普通实现任务里要求或暗示“开 researcher/tester/reviewer 子 Agent”。同一任务包的共享核心代码默认由一个实现 Agent 负责，避免多人同时改同一核心文件。
只有存在真正独立工作流、作者/Reviewer 独立性，或实现完成后的独立验证可以明显减少墙钟时间且重复 token 成本可接受时，才明确启用额外 Agent。禁止递归 subagent fan-out。

7：执行中发现新问题怎么处理
Agent 可以在同一 PR 内处理：与已知问题共享同一根因、当前任务验收必须修的 blocker、或当前改动直接造成的回归。
只是路过看到、但与当前任务独立的问题：记录证据并报告 coordinator，不自行扩大任务包。
不要把“顺手修”理解成“顺手重构”。

8：一级结构
使用 `1：标题`、`2：标题`、`3：标题` 组织任务。只在一级编号之间留一个空行。同一编号内部默认不留空行。

9：去掉装饰和重复
禁止无意义分隔符、整份任务代码围栏、重复的冻结边界和同义提醒。相同约束只写一次。能一行表达就不要拆成三行。
优先用**正向 Scope + 保持不变的契约**表达边界，例如“本轮仅修改 Editor interaction / Inspector；保持现有 Schema、Geometry、PPTist、render contracts 不变”。不要把同一边界拆成一长串“不要改 A / 不要改 B / 不要改 C”，除非某个禁止项本身就是已知高风险或必须单独阻断。过多否定式约束可能成为模型的验证锚点，诱发无价值的“证明自己没做这些事”的检查。

10：不能为了省 token 丢精度
仓库、branch、commit SHA、文件路径、命令、字段名、Schema/Contract、错误码、关键验收数字、模型/Reasoning 等必须准确。节省的是重复和仪式，不是技术边界。

11：测试写“风险覆盖”而不是测试清单竞赛
列出能证明当前改动/任务包的最小测试集合。任务包里多个问题共享同一个昂贵 Product/E2E 路径时，可以一次运行、用多个场景/断言分别证明各项验收，不要机械为每个小问题重复完整环境启动和全链路测试。
只有改动跨 Contract、持久化、最终渲染、文件/进程生命周期等边界时，才要求对应真实 E2E/回归。全量测试必须有明确 blast-radius 理由。

12：任务变大时只拆真正独立的部分
如果执行中发现一个任务包变成多个互不依赖的新工作流，Agent 应停止扩大那些独立范围，记录已完成/阻塞/建议拆分。
不要反过来把 coordinator 已经确认的同链路任务包重新拆成多个重复 Agent session，只因为里面有多个 Bug。

13：停止条件
任务单必须让 Agent 知道何时结束：当前 Scope/任务包的所有验收通过、无 blocking evidence、交付信息写入 PR 后就停止。不要要求它继续寻找“还能顺便优化什么”。

14：PR 交付
GitHub 工作遵循 `pr-delivery`。完整报告写进 PR Description 或顶层 comment；聊天只保留规定的一行成功/失败确认。不在任务末尾再复制一遍全部验收规则。

15：发送前自查
确认：这是一个单独结果或边界清楚的同链路任务包；打包决策由 coordinator 完成；没有无边界调查；没有默认 subagent；Reasoning 与风险匹配；共享测试没有无意义重复；每个 bundled item 仍有明确验收；完整规格没有被重复 brainstorm；达到验收后有明确 stop condition。

16：最小格式示例
模型：GPT-5.6 Terra　Reasoning：Medium

1：基线
Repo：owner/repo，main = `<SHA>`，Branch = `agent/task-branch`，Draft PR 到 main，不要 merge。

2：目标
完成本轮一个清晰工程结果。若这是任务包，列出 2～5 个已经确认、同链路的问题，并分别写一句验收。

3：实现边界
复用现有入口；共享链路只调查一次；不做列出的独立相邻功能；默认一个实现 Agent，不扫描 backlog 找额外工作。

4：验收
运行能证明本次改动的 focused tests；共享 E2E 可以一次覆盖多个 bundled item，但每项都要有对应证据。若改动真实跨越某条 Contract/E2E，再补那条路径。验收通过后停止。

5：交付
commit、push、更新 PR；完整报告写进 PR。
