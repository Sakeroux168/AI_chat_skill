# Self-test scenarios

Each scenario is walked through BOOTSTRAP → chat-orchestrator routing and the
routed skill. Expected behavior is normative: a skill change that breaks an
expected result needs a conscious decision, not a silent edit.

| # | Input | Expected |
|---|---|---|
| 1 | "我想做一个自动赚钱的软件" | Orchestrator routes to `requirement-grill` (vague product idea). Grill opens with 3–6 highest-value questions (what does it do, for whom, what does "赚钱" concretely mean), not a questionnaire. |
| 2 | "README 把 tezt 改成 test" | No grill. Clear one-line fix; orchestrator routes to nothing; done under basic engineering care. |
| 3 | 机械补 tests（明确规格） | `model-routing`: Luna with default Max (user override always wins). Declared as 模型：Luna　Reasoning：Max before work starts. |
| 4 | 复杂 Schema Migration | `model-routing`: Sol High (contract-class). Declared before start; not defaulted to Sol for other tasks. |
| 5 | 陌生仓库跨模块 debug | `model-routing`: Terra High/XHigh. Declared before start. |
| 6 | Agent 完成 GitHub 工作 | `pr-delivery`: full report written into PR description/comment; chat reply is exactly 已写进 PR #XX，无需向 GPT 复述。 — or the failure line if delivery failed. |
| 7 | Project Contract 与 Global Skill 冲突 | Precedence: Project Contract wins (BOOTSTRAP §precedence). Global conflict noted for fixing here. |
| 8 | GUI 改字体 | `gui-acceptance`: flagged 真人视觉验收; automated screenshot may be attached as evidence only; no "visually verified" claim. |
| 9 | 纯 backend bug | No human GUI acceptance required; standard review + tests suffice. |
| 10 | 用户说"你决定" | Recorded as DELEGATED in the Decision Sheet / task notes; agent decides and records the resolution; user is not re-asked. |

## Results (V1)

All ten scenarios were walked against the V1 texts above:

- 1–2: trigger gate in `requirement-grill` + orchestrator routing table produce
  grill vs no-grill correctly; round-size rule (3–6) prevents a dump.
- 3–5: `model-routing` table covers all three classes; declaration rule forces
  模型/Reasoning disclosure; Luna default prevents Sol-defaulting.
- 6: `pr-delivery` hard rule + fixed acknowledgment strings.
- 7: BOOTSTRAP precedence list is explicit and ordered.
- 8–9: `gui-acceptance` two-branch rule with borderline guidance.
- 10: DELEGATED semantics defined in `requirement-grill` rules.

No scenario produced a mechanical questionnaire, an undisclosed model choice,
or a self-reported visual verification. PASS.
