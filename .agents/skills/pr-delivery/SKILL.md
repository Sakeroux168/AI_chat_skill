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

## Protocol

1. Finish the work: commit, push, open the PR (Draft unless declared ready).
2. Write the complete report into the PR description. Required sections are
   task-dependent, but at minimum: what was done, how it was verified (real
   execution output, not claims), scope, risks/open questions, and any
   review-readiness statement requested by the user.
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
