# Self-test scenarios

Each scenario is walked through BOOTSTRAP → chat-orchestrator routing and the routed skill. Expected behavior is normative: a skill change that breaks an expected result needs a conscious decision, not a silent edit.

| # | Input | Expected |
|---|---|---|
| 1 | "我想做一个自动整理素材的软件，先帮我想清楚需求" | Orchestrator routes to `requirement-grill`; coordinator asks only load-bearing questions, not a fixed questionnaire. |
| 2 | "README 把 tezt 改成 test" | No grill; local low-risk change under `engineering-discipline`, one agent, focused verification. |
| 3 | 机械补 tests（明确规格） | `model-routing`: Luna Low/Medium, not Max by default; Model + Reasoning declared before work. |
| 4 | 普通范围明确的跨模块功能 | `model-routing`: Terra Medium by default; higher effort requires evidence. |
| 5 | 一个困难但范围明确的 bug，需要旗舰模型 | `model-routing`: Sol Medium first; High only after focused evidence of need. |
| 6 | 复杂 Schema / Contract / process lifecycle change | `model-routing`: Sol High; structural/high-risk evidence and relevant integration path required. |
| 7 | 极难 blocker / critical final gate | Sol XHigh may be justified; Max requires explicit quality-first justification and is not a routine default. |
| 8 | Agent 完成 GitHub 工作 | `pr-delivery`: full report written into PR description/comment; chat gives only the required compact completion/failure line. |
| 9 | Project Contract 与 Global Skill 冲突 | Project Contract wins; global conflict is recorded for later correction. |
| 10 | GUI 改字体 | `gui-acceptance`: human visual verification required; automation cannot self-award visual PASS. |
| 11 | 用户说"你决定" | Requirement decision is DELEGATED; user is not repeatedly asked. |
| 12 | "把这份完整 Issue 发给 Codex，别浪费 token" | No requirement grill. Orchestrator routes to `model-routing` + `agent-task-dispatch`; dispatch preserves the spec but does not re-brainstorm/rewrite it into Architecture + Plan. |
| 13 | 小任务但 Agent 想开 researcher/tester/reviewer 三个 subagent | `agent-routing` / `engineering-discipline`: reject fan-out; one implementation agent is default. |
| 14 | 中等任务执行中发现两个互不依赖的额外问题 | Current task stops expanding; non-blockers become follow-ups or a proposed split rather than silently consuming the remaining context. |
| 15 | 已达到 Scope 验收，但 Agent 还想继续清理邻近代码 | `engineering-discipline`: stop when scoped acceptance passes and no blocking evidence remains. |
| 16 | "用一个专业 Skill 帮我做商品海报 Prompt" | Orchestrator queries `AI_shared_skills` by metadata, loads only one matched active/trusted skill, and does not preload the registry. |

## Results target

A conforming run must preserve these invariants:

- vague product discovery can still be grilled when the coordinator/user actually requests it;
- complete implementation specs are not re-interviewed or re-brainstormed;
- ordinary implementation begins at a balanced reasoning level rather than High/Max by habit;
- Sol High is reserved for actual structural risk or demonstrated difficulty;
- Max/multi-agent execution is opt-in, not a normal safety ritual;
- small/moderate tasks default to one implementation agent;
- dispatch prompts have one main deliverable, bounded investigation, focused tests, and an explicit stop condition;
- unrelated findings become follow-ups instead of drive-by scope expansion;
- formal review, human visual acceptance, project precedence, and professional-skill discovery remain unchanged.
