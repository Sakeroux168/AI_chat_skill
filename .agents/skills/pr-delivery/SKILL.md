---
name: pr-delivery
description: Agent completion reports belong in the PR description or top comment; chat gets a one-line acknowledgment.
---

# PR Delivery

**Hard rule:** an agent's full completion report must be written into the
**PR Description** (or, if the PR already exists, a **top-level PR comment**).
A report that lives only in the chat window does not exist for reviewers,
future sessions, or GPT-side orchestration — and it forces the user to
manually copy dozens of lines between tools, which is exactly what this rule
prevents.

## Completion states

- **Implementation Complete:** scoped implementation and agent-executable
  acceptance are complete with no known implementation blocker; stop expanding
  implementation work.
- **Agent Delivery Complete:** the authorized PR report is written and its
  delivery verified; the agent may end its run.
- **Human Visual PENDING:** human acceptance is outstanding. Record that status
  without repeating screenshots, research, review, or cleanup merely to wait.
  It does not prevent Agent Delivery Complete and does not grant human
  acceptance, readiness, or merge approval. Explicit human-gated downstream
  actions remain blocked until their required approval is obtained.

These states do not authorize additional writes, publication, or merging.

## Protocol

1. Finish the work: commit, push, update the existing PR or open one when
   needed (Draft unless declared ready).
2. **Complete != exhaustive.** Write what changed, why, what was actually
   checked/run, its result, and what remains unverified / UNKNOWN. Summarize
   evidence accurately; raw logs and a section per item are not required.
   Preserved/out-of-scope subsystems need individual report entries only
   when they are a real risk surface of this change. Include any requested
   readiness statement or project-specific evidence. One canonical completion
   report in the PR satisfies this delivery obligation; reuse valid evidence
   under `code-review` without restarting implementation, tests, or review merely
   to publish the report. A separately requested review records its own findings
   and may reference the existing report/evidence rather than duplicate them.
3. Verify the write landed (re-fetch or confirm the tool returned the PR URL).
4. Final chat reply, exactly:
   - Success: `已写进 PR #XX，无需向 GPT 复述。`
   - Failure: `未写进 PR，需要向 GPT 转述。` — plus what blocked it.

## Rules

- Never fabricate verification output to fill the report; state blockers honestly.
- Do not paste the whole report into chat "for convenience" — the one-line
  acknowledgment is the deliverable in chat.
- If the platform cannot reach GitHub, say so immediately rather than
  simulating delivery.
- Project-specific report templates override the minimum here.
