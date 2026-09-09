---
name: code-review
description: Review PRs and agent output with P0–P3 priorities, evidence-backed findings, and risk-proportional reporting.
---

# Code Review

A short review with one substantiated blocker beats a list of nits. Do not
manufacture findings; "no blocking findings" is legitimate.

## Priority order (work top-down)

| Priority | Class | Examples |
|---|---|---|
| **P0** | Data loss / corruption / unrecoverable / security | Silent field drops on save; destructive migration; committed secrets |
| **P1** | Correctness / contract break / lifecycle / state pollution | Previous session's state leaking in; boundary bypassed; frozen contract violated |
| **P2** | Regression / compatibility / boundary | Old inputs no longer work; bad edge input accepted |
| **P3** | Maintainability | Duplication, dead abstraction, unclear naming |
| last | Style / nit | Formatting, wording |

Skip anything an automated gate already enforces; report what it cannot see.

## What must actually be examined

- The real diff (establish true base/head first) plus enough surrounding code
  to understand the design
- Tests: which runtime path each one exercises — mapping test ≠ integration test
- Failure and cleanup paths when the diff or direct dependencies affect them
- Scope: unrelated refactors and ownership violations flagged
- Docs vs implementation consistency

**Never accept the author's unsupported PASS claim as evidence.** Independently
inspect the diff and match verifiable execution output to the reviewed commit,
relevant environment, and claimed coverage, including its provenance. Reuse
valid evidence; changing from author to reviewer is not itself a reason to
rerun expensive tests. Run focused verification when evidence is missing,
stale, suspect, or insufficient, and preserve project-required independent
runs. Independent review of the diff and evidence remains required.

## Report format

Scale review depth to the actual diff under `engineering-discipline`.
Use only applicable sections below; preserved boundaries do not create
bespoke verification or a checklist of untouched subsystems.

```markdown
# Review Result
Verdict: PASS | PASS WITH FOLLOW-UP | BLOCK
## Blocking Findings        # severity, location, problem, why, evidence, minimum fix — or "None"
## Non-blocking Findings    # only real value; no filler
## Tests / Evidence Checked # which tests, which layer, what remains unproven
## Human / Visual Verification Required   # when the diff affects visual output/interaction
## Scope Check              # unrelated refactor / ownership violation / hidden scope growth
```

Rules: every finding carries evidence (quote line/value/output); "tests
passed" alone is not evidence; never claim visual verification you did not
perform; do not inflate nits or soften data-loss bugs. When receiving review
of your own work, verify each claim and fix or rebut on technical grounds —
never agree performatively.
