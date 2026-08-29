---
name: engineering-discipline
description: Universal rules for implementing, debugging, and refactoring — look up instead of guess, match rigor to risk, make the minimum correct change, test the real path.
---

# Engineering Discipline

How implementation work is done — applies from the moment a task is picked
up: reading code, choosing an approach, writing the change, proving it,
reporting it.

Core principle: **small things: restraint; big things: rigor. Match engineering
effort to actual risk.** Prefer the minimum correct implementation for the
current requirement. Do not add abstractions, compatibility layers, extension
points, or frameworks for hypothetical future needs when the existing
architecture can solve the real problem correctly.

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

## Scope and risk discipline

Classify the **current change**, not the whole project. A large project can
contain a low-risk local edit; a small project can contain a structural change.
Use actual risk — impact radius, failure cost, reversibility, and verification
difficulty — rather than code size or keywords alone.

### Local / reversible / low-risk change

Typical examples: copy changes, fixed styling, small UI adjustments, one local
interaction, a clearly bounded reversible feature, or simple logic that does
not cross module boundaries.

Default behavior:

- implement the minimum correct change in existing code;
- reuse existing structures before adding new ones;
- do not introduce a new Manager, Registry, Strategy, Adapter, compatibility
  layer, plugin system, or generic framework for a one-off need;
- do not refactor unrelated code while touching the area;
- test the changed behavior and directly affected paths;
- do not run broad regression suites unless evidence shows the impact is wider
  than initially believed.

### Structural / high-risk change

Raise rigor when the change materially affects contracts, schemas/data formats,
persistence, rendering/final-artifact correctness, file writes or publishing,
FFmpeg/codec behavior, IPC/RPC, process lifecycle, concurrency/cancellation,
crash/restart recovery, permissions/security boundaries, irreversible actions,
or shared core behavior with a wide blast radius.

For these changes:

- confirm ownership, existing contracts, and current consumers first;
- define inputs, outputs, invariants, and failure semantics explicitly;
- keep failure visible rather than silently degrading correctness;
- add direct regression coverage for the changed contract/behavior;
- run real integration/E2E paths when lower-level tests cannot prove the risk;
- preserve human verification when visual/pixel/final-output correctness cannot
  be established by automation alone;
- require explicit user authorization before irreversible actions.

## Abstraction and refactoring rule

A new abstraction or refactor must solve a **current demonstrated problem**.
These reasons alone are not sufficient:

- "more elegant";
- "more standard";
- "more architecturally pure";
- "more unified";
- "we may extend this later";
- "there might be a second implementation someday".

If the existing architecture can satisfy the current requirement correctly,
prefer reuse. Do not build a Registry for one mapping, a Strategy for two
branches, a Manager around one module, or a compatibility layer for an
unsupported hypothetical future.

If you discover an unrelated problem while working, record it as a follow-up
with enough evidence to act later. Do not fix it in the current diff unless it
actually blocks the requested change.

## Testing scope

Default: **test this change plus directly affected paths.** Expand regression
only when dependency inspection or observed behavior shows a wider impact, or
when the change is structural/high-risk as defined above.

Do not run the entire world for a small local edit. Do not skip core-path
verification merely to save time on a structural change.

## Decision shortcut

Before coding, ask:

1. Is this local, reversible, and low-cost to retry? If yes, prefer the minimum
   implementation and focused verification.
2. Does it materially affect contracts, data, persistence, rendering/final
   output, lifecycle/processes, concurrency, security, or irreversible
   behavior? If yes, raise evidence and regression depth.
3. Is the proposed abstraction solving a real current constraint or only a
   future possibility? If only future, do not add it.
4. Is the proposed refactor required to complete the request correctly? If
   not, leave it out and record a follow-up if useful.

## Human confirmation boundary

Ask a human when the answer is not in the code and getting it wrong is expensive:
business meaning nothing defines; architecture/boundary changes; persisted
format or migration behavior changes; irreversible actions (merge, force-push,
delete, publish); security/license/compliance calls.

Do not ask what you can look up or run yourself.

If genuinely blocking, do the non-dependent parts first, then ask once with
options and a recommendation.
