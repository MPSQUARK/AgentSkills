---
name: code
description: >-
  Use when: writing or modifying code; refactoring; implementing features;
  user invokes /code. Enforces senior-level SOLID, DRY, and Uncle Bob Clean
  Code — guard clauses, SRP, no duplication, production-review quality.
disable-model-invocation: true
---

# Senior Code Quality (SOLID, DRY, Clean Code)

Produce code a senior engineer would approve in production review.

## Think first
- Brief plan: responsibilities, abstractions, boundaries — then code.
- Stepdown rule: top of file/method reads as a narrative; details descend below.
- Prefer small composable units over monolithic functions/classes.

## Structure
- SOLID: one responsibility per function/class; ~20–30 lines max unless justified.
- One abstraction level per function — don't mix orchestration with low-level details.
- Names reveal intent and are searchable — never `data`, `d`, `stuff`, `handleThing`.
- One word per concept across the codebase (`Get` vs `Fetch` for the same idea — pick one).
- DRY: extract on second use; centralize shared behavior (helpers, value objects, static renderers).
- Named constants over magic numbers/strings (`MaxRetryCount`, `ThemeTokens.Primary`, not bare literals).
- Max nesting depth 3; replace long if/else with named helpers, maps, or strategy objects.
- **Avoid the `else` keyword** — use guard clauses and early `return`/`continue` so the happy path stays flat and left-aligned.
- Orchestrator methods delegate (`Draw` → `DrawNodes` → `GetNodeStyle`); they don't implement everything.
- Command-query separation: methods either change state OR return a value — not both.
- Prefer ≤2 parameters; no boolean flag arguments (use separate methods or an options object).
- Law of Demeter: don't chain through strangers (`a.GetB().GetC().Do()` — encapsulate or inject).
- Minimize side effects; callers should know what state changes.

## When modifying code (Boy Scout Rule)
- Leave code cleaner than you found it.
- Read surrounding code; match existing conventions.
- Refactor nearby structure if a patch would add duplication or hacks.
- Remove dead properties/code; don't layer workarounds on redundant state.
- Prefer enriching the canonical model over parallel DTOs.
- Extract try/catch — don't blend error handling with business logic.

## Red flags — refactor before submitting
- Copy-pasted blocks or scattered low-level primitives (inline canvas/API calls vs shared `Renderer`)
- Functions mixing unrelated concerns (layout + styling + drawing + hit-testing in one method)
- Duplicate state that already exists on a model
- Long parameter lists, flag booleans, or train-wreck chains
- `else` branches where a guard clause + early return would flatten the method
- Large files with no section structure
- Clever/condensed code over readable code
- Comments explaining *what* code does (rename/refactor instead)

## Output
- Plan → clean code → short decision summary.
- Ask when ambiguous; don't guess.

Comments only for non-obvious *why*, warnings, or constraints — not narration.
