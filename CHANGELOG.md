# CHANGELOG

## V1.1.1 (2026-08-26)

- Added lazy, metadata-driven discovery of professional capabilities through
  the separate `Sakeroux168/AI_shared_skills` registry.
- Kept the V1.1 global collaboration registry at exactly ten skills.
- Added no domain-to-skill hard-coded routes and no startup preload.

## V1.1 (2026-08-25)

- Added `agent-task-dispatch` as the tenth global skill.
- Agent task prompts now default to numbered top-level sections, one blank line only between top-level sections, no decorative separators, no unnecessary Markdown headings, and no repeated constraints.
- Codex dispatch now combines `model-routing` with `agent-task-dispatch`; other agents use `agent-task-dispatch` directly.
- Technical exactness remains mandatory: paths, SHAs, commands, schema/contract versions, field names, model names, and acceptance values are never shortened for token savings.
- README translated from English to Chinese for easier direct reading.
- Added a self-test scenario for compact agent task dispatch formatting.

## V1 (2026-08-25)

Initial release. Nine skills, derived by generalizing the AI-Reveal-Video-Studio
project skills (`ai-reveal-requirement-grill`, `ai-reveal-engineering-discipline`,
`ai-reveal-code-review`) plus rules that previously lived only in chat
convention (PR delivery protocol, project hygiene, GUI acceptance, agent and
Codex model routing).

- BOOTSTRAP.md as the single lazy-load entry point.
- Explicit precedence: user instruction > project contracts > global skills > defaults.
- chat-orchestrator routes; all other skills load on demand only.
