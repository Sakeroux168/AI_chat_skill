---
name: engineering-discipline
description: Universal rules for implementing, debugging, and refactoring — inspect before guessing, match rigor and resource use to actual risk, make the minimum correct change, test the real path, and stop when scoped acceptance is satisfied.
---

# Engineering Discipline

How implementation work is done from task pickup through delivery.

Core principle: **small things: restraint; big things: rigor. Match engineering effort, reasoning, investigation depth, and testing to actual risk.** Prefer the minimum correct implementation for the current requirement. Do not add abstractions, compatibility layers, extension points, frameworks, or process ceremony for hypothetical future needs when the existing architecture can solve the real problem correctly.

## Mandatory workflow

1. **Inspect before assuming.** Read the real implementation, relevant types, and at least one direct call site before relying on a function/API/schema field/flag/protocol.
2. **Separate facts from decisions.** Discoverable repo facts are looked up; only unresolved business/architecture decisions are escalated.
3. **Search before creating.** Reuse an existing helper/entry point/state owner before inventing a new one.
4. **Reproduce before modifying defects.** Capture the observed failure and relevant values before the fix.
5. **Make the minimum change.** Fix the requested root cause or coordinator-defined task bundle and nothing else. Unrelated cleanup becomes a follow-up.
6. **Test the real risk.** Use focused tests by default; integration/E2E only when lower layers cannot prove the behavior that changed.
7. **Fail loud when correctness is at risk.** Do not hide data loss, wrong output, stale state, or contract failure behind fallback behavior.
8. **Check lifecycle where relevant.** Look for stale work, previous-session pollution, cleanup failures, and omitted fields inheriting old values when the change actually touches those paths.
9. **Never weaken a test merely to get green.** If a test is wrong, prove that separately.
10. **Report honestly.** Distinguish observed from inferred and state what was not verified.
11. **Stop when done.** Once the scoped acceptance criteria pass and no blocking evidence remains, do not continue searching for adjacent improvements merely because more work is possible.

## Scope and risk discipline

Classify the **current change**, not the whole project. Use impact radius, failure cost, reversibility, and verification difficulty.

### Local / reversible / low-risk

Examples: copy/style changes, one bounded UI behavior, a narrow bug with a reproduced cause, deterministic docs/tests, or simple logic that does not cross contracts/persistence/process boundaries.

Default behavior:

- use the existing code path;
- one implementation agent is enough;
- do not spawn research/test/review subagents by default;
- do not create architecture or planning documents unless the task actually needs a durable design decision;
- investigate the target implementation plus direct callers/consumers/tests first;
- run focused verification only;
- stop when the requested behavior is proven.

### Moderate / bounded

Examples: a clear feature touching several files or modules without changing a core contract.

Default behavior:

- keep one coherent deliverable or coordinator-defined task bundle;
- inspect only the direct dependency chain unless evidence expands the blast radius;
- prefer one implementation agent;
- use broader integration tests only for paths the change actually crosses;
- split only when work has become genuinely independent rather than mechanically splitting every bug or ticket.

### Structural / high-risk

Raise rigor when the change materially affects contracts, schemas/data formats, persistence, rendering/final-artifact correctness, filesystem writes/publish, FFmpeg/codec behavior, IPC/RPC, process lifecycle, concurrency/cancellation, crash/restart recovery, security boundaries, irreversible actions, or shared core behavior with a wide blast radius.

For these changes:

- confirm ownership/contracts/invariants/current consumers first;
- define failure semantics explicitly;
- add direct regression coverage for the changed contract/behavior;
- run the relevant real integration/E2E path when lower-level tests cannot prove it;
- preserve human verification for visual/interaction/final-output judgments;
- use an independent reviewer when contract freeze or irreversible architecture merits separation of author and reviewer;
- require explicit user authorization before irreversible actions.

## Cohesive task bundling

**Task bundling is coordinator-owned by default.** The coordinator/Chat should group already-known work before dispatch so implementation-agent quota is spent on implementation and verification rather than on scanning the entire backlog for more work.

A dispatch may intentionally contain several small or medium fixes when they form one coherent engineering bundle. Prefer bundling when the items:

- sit on the same module or direct runtime/data-flow chain;
- require substantially the same implementation context;
- share most of the setup, integration path, or verification cost;
- are already confirmed defects/requirements rather than speculative cleanup;
- retain clear per-item acceptance inside one bounded delivery.

“One task” means **one coherent engineering outcome**, not necessarily one bug ticket.

Do not bundle merely because code is nearby. Keep work separate when it introduces a distinct product/architecture decision, schema or migration, security boundary, irreversible operation, unrelated performance program, or materially different risk class.

Implementation-agent behavior inside a bundle:

- own the shared code path coherently rather than splitting same-core-file edits across multiple agents by default;
- fix a directly shared root cause, a blocker required for the bundle's acceptance, or a regression caused by the current change when needed;
- report newly discovered adjacent but independent issues back to the coordinator instead of silently expanding scope;
- do **not** scan the backlog/repository looking for additional bundle candidates unless the coordinator explicitly asked for that research;
- reuse shared test setup where appropriate, but preserve enough evidence to prove each bundled acceptance condition.

Parallel agents are optional, not automatic. They are most useful for truly independent workstreams or bounded post-implementation verification whose wall-clock benefit exceeds duplicated context/token cost. Do not use several agents to edit the same core path merely because parallelism is available.

## Investigation budget

Investigation is evidence-driven, not open-ended.

Start with: **task/Issue → target implementation → direct call sites/consumers → directly affected tests**.

Expand only when one of those produces concrete evidence of another dependency, unknown, or blocker. Do not recursively read every nearby module, historical document, or unrelated test suite “for completeness”. A complete task spec is implementation input; do not automatically re-brainstorm or rewrite it into a second spec.

If investigation, token use, context growth, or elapsed time becomes disproportionate to the apparent task:

1. re-check the requested scope and acceptance criteria;
2. identify which discovered work is actually blocking;
3. defer non-blocking independent findings;
4. preserve a coherent coordinator-defined same-chain bundle;
5. split only the newly independent jobs rather than silently expanding or mechanically fragmenting the bundle.

## Subagent rule

Implementation subagent count defaults to **zero** for local and moderate work. Use additional agents only when there is a concrete independent workstream or an independence requirement (for example author vs final contract reviewer). Never recursively spawn agents merely to be thorough.

For a coordinator-defined task bundle, one implementation owner is normally preferred for the shared code path. Bounded independent test/QA agents may run in parallel when that materially reduces elapsed time without duplicating the same implementation work.

## Abstraction and refactoring rule

A new abstraction/refactor must solve a **current demonstrated constraint**. “Cleaner”, “more standard”, “future-proof”, “we may extend this later”, or “another implementation may exist someday” are not sufficient reasons.

Do not build a Registry for one mapping, a Strategy for two branches, a Manager around one module, a second state store beside an existing source of truth, or a compatibility layer for an unsupported hypothetical future.

## Testing scope

Default: **test this change/task bundle plus directly affected paths.** Expand only when dependency inspection, observed behavior, or structural/high-risk impact proves a wider regression surface.

Broad regression is not automatically more rigorous. For a small local edit it may be wasted time/tokens; for a contract/lifecycle/rendering change it may be necessary. Choose based on the behavior that could actually break.

When bundled fixes share an expensive integration/E2E setup, prefer one shared run with distinct scenarios/assertions for the bundled acceptance conditions rather than repeating effectively identical setup for each micro-fix.

## Human confirmation boundary

Ask a human when the answer is not in code and getting it wrong is expensive: undefined business meaning, architecture/boundary changes, persisted format/migration semantics, irreversible actions, security/license/compliance decisions, or true product preferences.

Do not ask what can be looked up or run.

If genuinely blocked, complete non-dependent work first, then ask once with options and a recommendation.
