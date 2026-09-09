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

Before ending a task, internally check only for material, durable information
from the current Scope that remains unrecorded. Reuse existing code, tests,
docs, or the PR report; record a missing in-scope fact once in an appropriate
home above. Do not reopen investigation or expand Scope to create knowledge
artifacts. Independent Skill extraction or general-method organization becomes a
follow-up. Explicit in-scope and project documentation requirements still apply.

## Scratch space

Experiments and throwaway tests belong outside the repo (a local temp
directory), unless they graduate into real tests.
