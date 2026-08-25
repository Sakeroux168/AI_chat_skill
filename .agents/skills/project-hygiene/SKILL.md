---
name: project-hygiene
description: What never gets committed, and where long-term knowledge belongs.
---

# Project Hygiene

## Never commit long-term (blockers in review)

- `node_modules/`, build output, dependency trees
- Caches, temp files, debug dumps, scratch scripts
- Secrets: API keys, tokens, passwords, credentials files
- Full chat logs / session transcripts
- Unnecessarily large binaries or data dumps

If one must be discussed in a PR, describe it and link — do not paste it into
the repo.

## Where long-term knowledge goes

| Knowledge | Home |
|---|---|
| Behavior | code + tests |
| Why decisions were made | docs / ADR / PR description |
| Frozen interfaces & data formats | contracts / schemas |
| Working methods that recur across tasks | skills (project-level, then promote here if universal) |

Knowledge that lives only in a chat dies with the chat. Before ending a task,
ask: is there anything learned here that the next session will need? If yes,
it goes to one of the homes above (or a project skill), not into a log file.

## Scratch space

Experiments and throwaway tests belong outside the repo (a local temp
directory), unless they graduate into real tests.
