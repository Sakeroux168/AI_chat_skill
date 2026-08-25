# CHANGELOG

## V1 (2026-08-25)

Initial release. Nine skills, derived by generalizing the AI-Reveal-Video-Studio
project skills (`ai-reveal-requirement-grill`, `ai-reveal-engineering-discipline`,
`ai-reveal-code-review`) plus rules that previously lived only in chat
convention (PR delivery protocol, project hygiene, GUI acceptance, agent and
Codex model routing).

- BOOTSTRAP.md as the single lazy-load entry point.
- Explicit precedence: user instruction > project contracts > global skills > defaults.
- chat-orchestrator routes; all other skills load on demand only.
