---
name: engineering-discipline
description: Universal rules for implementing, debugging, and refactoring — look up instead of guess, minimal change, test the real path.
---

# Engineering Discipline

How implementation work is done — applies from the moment a task is picked
up: reading code, choosing an approach, writing the change, proving it,
reporting it.

## Mandatory workflow

1. **Inspect before assuming.** Before calling a function/API/schema field/
   flag/protocol, read the real implementation, its types, and at least one
   existing call site. A plausible name is not evidence.
2. **Separate code facts from business decisions.** If the answer is
   discoverable in the repo, find it yourself. Escalate only what code cannot
   decide.
3. **Search before creating.** Before adding a function/class/endpoint/field/
   utility, search for an existing one. If you still add it, say in the PR why
   reuse was not possible.
4. **Reproduce before modifying.** For any defect, reproduce it and capture
   observed values *before* touching the fix. A fix with no reproduction is a
   guess with better formatting.
5. **Make the minimum change.** Fix the root cause and nothing else.
   Unrelated cleanup → follow-up task, not this diff. No drive-by refactors.
6. **Test the real runtime path.** Name which path each test exercises. A
   mapping/unit-level pass is not an integration pass; assertions must fail on
   the intended regression, not restate the implementation.
7. **Fail loud.** Prefer errors that surface over silent fallbacks that hide
   data loss or wrong behavior.
8. **Check lifecycle and state pollution.** Cleanup paths, stale state leaking
   across sessions/loads, "previous" values bleeding in when the new input
   omits a field.
9. **Never lower standards to get green.** Skipping/simplifying/weakening a
   test to make CI pass is a blocker, not a fix.
10. **Report honestly.** Distinguish observed from inferred; state what was
    NOT verified; if scope was skipped, say which part and why.

## Human confirmation boundary

Ask a human when the answer is not in the code and getting it wrong is expensive:
business meaning nothing defines; architecture/boundary changes; persisted
format or migration behavior changes; irreversible actions (merge, force-push,
delete, publish); security/license/compliance calls.

Do not ask what you can look up or run yourself.

If genuinely blocking, do the non-dependent parts first, then ask once with
options and a recommendation.
