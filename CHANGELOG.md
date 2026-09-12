# CHANGELOG

## V1.2 (2026-09-12)

- Added `coordinator-continuity` as the eleventh global collaboration skill.
- Project-specific coordinator memory now lives in each project repository, not in `AI_chat_skill`.
- Standardized the lightweight project continuity pattern:
  - rolling `.ai/coordinator/MEMORY.md` for current long-term coordinator knowledge;
  - current `.ai/coordinator/HANDOFF.md` for immediate execution state.
- `MEMORY.md` is a rewriteable snapshot rather than an append-only log; still-relevant decisions and ideas remain in the latest version, while superseded/obsolete material may leave the current snapshot and remain recoverable through Git history.
- Added an event-driven Memory Checkpoint before project task dispatch: update memory only when durable project knowledge actually changed.
- Explicitly rejected message-count polling such as rereading memory every N chat messages; read once and reuse while current, refresh only when stale, uncertain, or entering a new coordinator session.
- Coordinator owns MEMORY/HANDOFF by default; implementation agents continue to report through Issue/PR delivery instead of filling project memory with execution logs.

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
