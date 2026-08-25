---
name: requirement-grill
description: Interrogate a vague requirement in adaptive rounds until a Requirement Decision Sheet is complete.
---

# Requirement Grill

Before architecture or implementation of a significant new feature, dig out
what the user has *not* said: real goal, workflow, hidden assumptions,
boundaries, state lifecycle, recovery, batch semantics, failure strategy,
acceptance criteria, explicit non-goals — so implementation never invents
business rules on the user's behalf.

Cross-harness by design. Semantic trigger phrases ("先 grill 我", "grill me on
this", "$requirement-grill"), not harness slash commands.

## When to trigger

- New product workflow / end-to-end flow / "help me design the whole process"
- Schema/contract changes, persistence, recovery, background task lifecycle
- Batch processing, multi-agent boundaries, complex UI state
- Vague openers: "我想做一个……", "加一个新功能……", "自动……", "批量……",
  "智能……", "类似某某软件……"

**Do NOT grill:** small bug fixes, one-line changes, text tweaks, work with a
complete spec or approved Decision Sheet. If unsure, ask exactly one gate
question ("这个改动是否引入新的状态、持久化或批量语义？") and proceed.

## Rules (priority order)

1. **Never dump a questionnaire.** Open with only the 3–6 questions most
   likely to change the solution. Number them; recommend an answer each.
2. **Adaptive depth.** Each round's answers reshape the unknowns; ask the next
   round only about what is unknown AND load-bearing. Dependent questions wait.
3. New ambiguity introduced by an answer → follow up on that branch first.
4. "你决定 / 不在乎" = **DELEGATED**: record it, the architecture agent decides.
5. "不知道" = **UNKNOWN**: record honestly; never invent a business rule to fill it.
6. Facts discoverable in code/docs/schemas: the agent looks them up itself.
   Only decisions belong to the user.
7. **Stop by readiness, not count.**

## Five layers

1. **Goal** — what outcome does the user actually want? Success criterion?
2. **Workflow** — how will they actually operate it, step by step? Frequency?
   Which steps automated vs human-confirmed?
3. **Failure & edges** — item #37 fails: retry/skip/stop/manual? Timeouts,
   quota exhaustion, bad output quality? What must interrupt the user?
4. **Data / state / lifecycle / recovery** — who owns state and at what scope?
   App close / reboot behavior? Resume point and checkpoint granularity?
   Intermediate artifacts and cleanup? Batch scale (10 vs 100 vs 10000)?
5. **Acceptance** — concrete verification steps and thresholds; force out at
   least a few Explicitly Out of Scope items.

## Output: Requirement Decision Sheet

```markdown
# Requirement Decision Sheet: <name>
## Goal            ## User Workflow       ## Must Have
## Explicitly Out of Scope                ## Business Rules (USER/DELEGATED/UNKNOWN per rule)
## State / Lifecycle               ## Failure & Recovery
## Persistence      ## Acceptance Criteria
## Delegated Decisions    ## UNKNOWN    ## Architecture Questions
## Ready for Planning: YES | NO
```

NO → keep grilling. YES → proceed to Architecture → Plan → Implementation;
the sheet travels forward verbatim and implementation may not silently fill
in USER-marked rules or UNKNOWNs.
