# Skill Registry

V1 contains exactly nine skills. Nothing else. Extending this list requires a
PR and a strong reason — skills cost context every time they are loaded.

| Skill | Purpose | Loads |
|---|---|---|
| `chat-orchestrator` | Routes an incoming request to the right skill; owns nothing else | always |
| `requirement-grill` | Adaptive interrogation of vague requirements → Requirement Decision Sheet | on route |
| `agent-routing` | Role definitions and current default model/agent mappings | on route |
| `model-routing` | Codex task → model + reasoning-effort choice, with mandatory pre-work declaration | on route |
| `engineering-discipline` | How implementation work is done: look up, don't guess; minimal change; real-path testing | on route |
| `code-review` | PR review priorities (P0–P3), evidence rules, report format | on route |
| `pr-delivery` | Where agent completion reports must live (in the PR), and the chat acknowledgment protocol | on route |
| `project-hygiene` | What never gets committed; where long-term knowledge goes | on route |
| `gui-acceptance` | When human visual acceptance is required vs not | on route |

Cross-harness: all skills are plain Markdown with no runtime assumptions.
Any agent (ChatGPT, Codex, Claude, Hermes) reads them from this repo
directly or via raw.githubusercontent.com.

Versioning: see [CHANGELOG.md](CHANGELOG.md). V1 = initial set.
