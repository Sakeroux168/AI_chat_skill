---
name: requirement-grill
description: Coordinator-side requirement discovery for genuinely vague or load-bearing product ambiguity; do not rerun it for a coding task that already has a complete implementation spec.
---

# Requirement Grill

Purpose: before implementation of a genuinely ambiguous significant feature, uncover the business decisions the repository cannot answer. This is primarily a **coordinator / product / architecture-stage** skill, not a mandatory prelude for every Coding Agent.

## When to trigger

Trigger when either:

- the user explicitly asks to brainstorm, grill, interrogate, or clarify the requirement before implementation; or
- the coordinator can name a **load-bearing unresolved business decision** whose answer materially changes workflow, state ownership, failure behavior, persistence, or acceptance.

Typical examples: a vague new product workflow, unclear persistence/recovery semantics, undefined batch failure policy, or a new contract whose business meaning is not decided.

## Do NOT trigger

Do not grill when:

- the task is a bug fix, local change, copy/style edit, or bounded implementation;
- an Issue/spec already states Goal + Scope + Acceptance + Explicitly Out of Scope;
- the Coding Agent merely sees words such as “自动”, “批量”, “智能”, or “新功能” inside an otherwise complete task;
- the missing information is discoverable in code/docs/contracts;
- asking broad questions would cost more than resolving the one actual ambiguity.

A Coding Agent receiving a complete implementation task should implement it. It must not reopen the whole feature just because this skill exists.

## Rules

1. **Ask only load-bearing questions.** Open with at most 3–6 questions most likely to change the solution; usually fewer is better.
2. **One missing decision means one question, not a new discovery phase.** If implementation reveals a single business ambiguity, report/ask that decision directly.
3. **Adaptive depth.** Follow up only on uncertainty that remains both unresolved and solution-shaping.
4. **“你决定 / 不在乎” = DELEGATED.** Record it and decide without repeatedly asking.
5. **“不知道” = UNKNOWN.** Record it honestly; do not invent a product rule.
6. **Look up facts yourself.** Code/docs/schema facts are not user questions.
7. **Stop by readiness.** Do not force every feature through every possible question category when the load-bearing decisions are already clear.

## Coverage areas, not ceremony

Use these as prompts when relevant, not as a mandatory five-stage workflow:

- Goal / success outcome
- User workflow / automation vs confirmation
- Failure and edge policy
- Data/state/lifecycle/recovery
- Acceptance / explicit non-goals

## Output

When a durable artifact is useful, produce a concise Requirement Decision Sheet containing only material decisions:

- Goal
- User Workflow
- Must Have
- Explicitly Out of Scope
- Business Rules (USER / DELEGATED / UNKNOWN)
- State / Lifecycle / Persistence where relevant
- Failure / Recovery where relevant
- Acceptance Criteria
- Remaining UNKNOWN / Architecture Questions
- `Ready for Implementation: YES | NO`

`NO` → resolve only the remaining blocking ambiguity.

`YES` → proceed using the existing architecture and task scope. **Do not mechanically require a separate Architecture phase and Plan document.** Create those only when the current change actually needs a durable architecture decision or multi-step plan.
