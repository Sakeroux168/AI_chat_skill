---
name: coordinator-continuity
description: Keep long-running project coordination continuous across Chat sessions by maintaining a rolling project memory plus a current handoff, without turning either into a transcript.
---

# Coordinator Continuity

Use this skill when a Chat/coordinator is continuing a project across sessions, returning to a project after a gap, or preparing to dispatch project work where project-level coordinator memory exists.

This skill defines **how** continuity is maintained. The actual project memory belongs in the project repository, never in `AI_chat_skill`.

## Project files

Preferred default paths:

- `.ai/coordinator/MEMORY.md`
- `.ai/coordinator/HANDOFF.md`

A project may define another location. Project-local conventions win.

### MEMORY.md

`MEMORY.md` is the coordinator's **current high-value long-term project memory snapshot**.

Keep only information that materially helps a future coordinator avoid re-discovery, wrong decisions, repeated mistakes, or loss of valuable product thinking. Typical contents include:

- durable product direction and constraints;
- current architecture/ownership decisions;
- still-active engineering rules or frozen boundaries;
- important verified technical facts;
- valuable future ideas that are intentionally deferred;
- important decisions and reasons that still matter.

It is **not append-only**. When updating it, rewrite and compact the current snapshot:

- add new durable knowledge;
- merge duplicates;
- replace obsolete discussion with the current conclusion;
- remove information that no longer needs to be carried in the latest snapshot;
- keep still-relevant decisions, ideas, constraints, and reasons even when they are old.

Do not preserve old material inside the latest file merely for history. Git history is the archive. Do not create a growing chain of versioned MEMORY files by default.

A useful test is:

> If the next coordinator does not know this, is it materially more likely to re-discuss the same thing, make a wrong decision, or lose a valuable idea?

If not, it usually does not belong in `MEMORY.md`.

Transient execution state, raw logs, ordinary command output, casual discussion, unverified guesses, and duplicated Issue/PR specifications do not belong there.

Where helpful, keep a short source pointer such as an Issue, PR, commit, or project document. Do not turn the file into a bibliography.

### HANDOFF.md

`HANDOFF.md` is the **current execution snapshot**, not long-term memory.

Keep the minimum information needed for the next coordinator to resume immediately, for example:

- current main/baseline;
- active Issue/PR;
- current branch/commit;
- current implementation owner;
- what is already confirmed;
- what remains unfinished;
- blockers;
- exact next step.

Rewrite it as the situation changes. Old handoffs remain available through Git history; do not accumulate an archive inside the latest file unless the project explicitly requires one.

## Read policy

Do not reread these files every N messages.

Read or refresh them when:

1. a new coordinator Chat starts on the project;
2. the coordinator returns to the project after a meaningful gap;
3. project continuity is uncertain or the files may have changed elsewhere;
4. a project task is about to be dispatched and the current session has not already loaded a known-current version.

If the current session already read the latest versions and no external change occurred, reuse that context rather than fetching them again.

## Memory checkpoint before project dispatch

Before dispatching a project task to Codex, Claude, Hermes, Ox, or another implementation agent, the coordinator performs a lightweight Memory Checkpoint:

1. check whether the current discussion produced new durable project knowledge;
2. if yes, update `MEMORY.md` before dispatch;
3. if no, leave `MEMORY.md` untouched;
4. refresh `HANDOFF.md` only when the material execution state changed.

This checkpoint is event-driven, not message-count-driven.

Do not modify MEMORY just to satisfy process. A no-change checkpoint should produce no commit.

## Session handoff

When the coordinator is about to leave a long session, context is becoming unreliable, or a natural project phase ends:

1. fold any pending durable knowledge into the latest `MEMORY.md`;
2. rewrite `HANDOFF.md` to the exact current execution state;
3. ensure transient guesses are not promoted to facts;
4. stop expanding work merely to make the handoff look complete.

The next coordinator normally reads:

`Global Skills → project MEMORY.md → project HANDOFF.md → active Issue/PR`

Historical MEMORY/HANDOFF versions are read only when the current snapshot or source references do not answer a historical question.

## Ownership

The Chat/coordinator owns these continuity files by default.

Implementation agents should not independently add execution logs, test chatter, or their own summaries to `MEMORY.md` or `HANDOFF.md` unless the coordinator explicitly assigns that work. Agents report through the normal Issue/PR delivery path; the coordinator decides what has durable value.

## Scope boundary

Project-specific memories, decisions, product ideas, and handoff state must remain in that project's repository. Do not copy them into this global-skill repository.

Global skills define the method; project repositories own the memory.
