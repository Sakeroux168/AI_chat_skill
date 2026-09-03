---
name: agent-task-dispatch
description: 派发 Codex、Claude、Hermes、Ox 等 Agent 任务时，用低 token、高可读性、单一明确交付物的格式编写任务单。
---

# Agent 任务派发规范

用途：把已经明确的任务整理成 Agent 可直接执行的任务单，同时控制上下文、推理和执行范围。它只规定派发表达与任务大小，不替代项目 Contract/ADR、`model-routing`、`engineering-discipline` 或真正需要的 requirement discovery。

1：一个任务一个主结果
默认每份任务只包含一个主要交付结果。不要把“调查 + 重新设计架构 + 实现 + 性能优化 + 全量回归 + 文档宇宙”打包成一个 Agent 任务。如果多个部分可以独立交付，拆成连续的小任务；如果必须一起完成，明确说明为什么它们是同一条不可分割的正确性链。

2：完整规格不重复 brainstorm
如果 Issue/任务已经包含 Goal、Scope、Acceptance、Out of Scope，就把它当实现输入。不要再要求 Coding Agent 重新头脑风暴、重新采访用户、重新写一份同义 spec 或强制走 Architecture → Plan 仪式。只有一个真正阻塞实现的业务决策时，只问/报告那个决策。

3：控制调查边界
默认要求 Agent 先读：任务/Issue、目标实现、直接调用者/消费者、直接相关测试。只有出现具体证据时才向外扩。不要用“全面调查整个仓库”“读所有相关文件”“穷举所有风险”这类无边界措辞，除非任务本身就是研究/审计。

4：模型与 Reasoning
派发 Codex 前加载 `model-routing`。在任务最前面写 `模型：XXX　Reasoning：XXX`。Medium 是普通实现的常见起点；High/XHigh/Max 必须由当前任务风险或已观察到的困难支持。不要因为项目重要、Prompt 很长或“更保险”就升档。

5：Subagent 默认关闭
不要在普通实现任务里要求或暗示“开 researcher/tester/reviewer 子 Agent”。需要独立并行工作流或作者/Reviewer 独立性时再明确启用。禁止递归 subagent fan-out。

6：一级结构
使用 `1：标题`、`2：标题`、`3：标题` 组织任务。只在一级编号之间留一个空行。同一编号内部默认不留空行。

7：去掉装饰和重复
禁止无意义分隔符、整份任务代码围栏、重复的冻结边界和同义提醒。相同约束只写一次。能一行表达就不要拆成三行。

8：不能为了省 token 丢精度
仓库、branch、commit SHA、文件路径、命令、字段名、Schema/Contract、错误码、关键验收数字、模型/Reasoning 等必须准确。节省的是重复和仪式，不是技术边界。

9：测试写“风险覆盖”而不是测试清单竞赛
列出能证明当前改动的最小测试集合。不要机械写 A～P 十几组场景来显得完整。只有改动跨 Contract、持久化、最终渲染、文件/进程生命周期等边界时，才要求对应真实 E2E/回归。全量测试必须有明确 blast-radius 理由。

10：任务变大时先拆
如果执行中发现一个中小任务实际上变成多个独立问题，Agent 应停止扩大范围，记录已完成/阻塞/建议拆分，而不是为了“完整交付”继续吞噬上下文和额度。

11：停止条件
任务单必须让 Agent 知道何时结束：当前 Scope 的验收通过、无 blocking evidence、交付信息写入 PR 后就停止。不要要求它继续寻找“还能顺便优化什么”。

12：PR 交付
GitHub 工作遵循 `pr-delivery`。完整报告写进 PR Description 或顶层 comment；聊天只保留规定的一行成功/失败确认。不在任务末尾再复制一遍全部验收规则。

13：发送前自查
确认：只有一个主结果；没有无边界调查；没有默认 subagent；Reasoning 与风险匹配；测试数量与 blast radius 匹配；完整规格没有被重复 brainstorm；相同约束没有重复；达到验收后有明确 stop condition。

14：最小格式示例
模型：GPT-5.6 Terra　Reasoning：Medium

1：基线
Repo：owner/repo，main = `<SHA>`，Branch = `agent/task-branch`，Draft PR 到 main，不要 merge。

2：目标
只完成本轮一个明确结果，说明输入、输出和用户可观察行为。

3：实现边界
复用现有入口；只调查目标实现和直接依赖；不做列出的相邻功能；默认不使用 subagent。

4：验收
运行能证明本次改动的 focused tests；若改动真实跨越某条 Contract/E2E，再补那条路径。验收通过后停止。

5：交付
commit、push、更新 PR；完整报告写进 PR。
