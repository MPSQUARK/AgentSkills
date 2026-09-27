---
name: code
description: >-
  Use when: writing or modifying code; refactoring; implementing features;
  user invokes /code. Senior SOLID/DRY/Clean Code — discovery-first, correctness
  before performance, production quality. Gate: run code-review before done.
disable-model-invocation: true
---

# Senior Code Quality

Produce code a senior engineer approves in production review. After implementation → run `code-review` self-review gate before marking done.

## Discover before you write

0. Read **tests** for similar code — behavior, edge cases, threading assumptions
1. Grep keywords (cache, scratch, buffer, accumulate, reduce, …)
2. Read 2–3 implementations in the **same niche** (layer + execution context)
3. Match **sibling APIs** — naming, errors, symmetry
4. State what you reuse vs why new code is justified

Before scratch buffers, caches, allocators, accumulators, or new types — check how this niche already handles them. Fix or flag the same mistake elsewhere; don't replicate it.

**Session regression**: if category X was corrected this session, grep for X before submitting new code ([principle catalog](../learn/references/principle-catalog.md)).

## Quality ordering

`Correctness → execution-context fit → structure/reuse → performance`

No benchmarking until correctness is established. Perf that adds shared mutable state or precision loss is a regression.

## Think first

- Plan responsibilities, abstractions, boundaries — then code
- Stepdown: file/method reads as narrative; details below
- Small composable units over monoliths
- For pivot discipline, root-cause fixes, and implicit quality bars, see `agent-reasoning`

## Fit the execution niche

Identify niche (UI thread, request handler, batch, device, hot loop) before structure. Read same-niche code; don't import idioms from a different niche. Match natural unit of work — don't serialize parallelizable work or parallelize sequential work.

## Authoring principles

| Principle | Rule |
|---|---|
| Immutable by default | Shared mutable state needs established pattern + justification |
| Invariants explicit | Document threading, ownership, aliasing, reentrancy where non-obvious |
| Validate at boundaries | Parse/validate at edges; trust typed values in hot paths |
| Fail fast | Clear errors at boundary — no silent coercion or swallowed exceptions |
| Illegal states unrepresentable | Types encode constraints, not boolean flags + sentinels |
| One error model per layer | Match siblings — don't invent a third style |
| YAGNI | No speculative hooks/factories unless `code-review` names a near-term need |
| Behavioral equivalence | Refactors preserve semantics; behavior changes are explicit + tested |

## Structure

- SOLID; ~20–30 lines per function unless justified; one abstraction level per function
- Searchable names; one word per concept; DRY — discover first, extract on second use
- Named constants; max nesting 3; guard clauses over `else`; orchestrators delegate
- CQRS; ≤2 params; no boolean flags; Law of Demeter; minimal side effects

## When modifying (Boy Scout)

Leave cleaner than found. Match conventions. Refactor if patch adds duplication. Remove dead code. Enrich canonical model over parallel DTOs. Extract try/catch from business logic. Smallest structure that works. Unfamiliar niche → read its docs and implementations first.

## Red flags — fix before submitting

**Structure**: copy-paste; mixed concerns; duplicate state; long params/flag booleans; train-wrecks; large unstructured files; clever over readable; comments explaining *what*; orchestration bigger than work; one-off abstraction duplicating existing pattern; duplicate code paths; embarrasses a domain specialist

**Agent anti-patterns**: happy-path-only; partial refactor (signature changed, call sites not); inconsistent sibling API; stringly-typed where types exist; speculative abstraction; silent failure (`catch {}`, default on error); fighting the type system (casts/`any`/null-forgiving); scratch/cache/accumulator without checking niche patterns

**Process**: validation/tooling in user's scratch dirs

## Output

Plan → clean code → short decision summary. Ask when ambiguous. Comments only for non-obvious *why*, warnings, constraints.
