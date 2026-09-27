---
name: agent-reasoning
description: >-
  Use when: starting any task, stuck, debugging, or marking work done. One rule:
  what would a human colleague do step by step? Covers diagnosis, pushback,
  research, verification, proven patterns, workspace hygiene.
---

# Agent Reasoning

## The rule

Before any non-trivial action, ask: **What would a competent human colleague do step by step to achieve this goal?** Then do that — not the fastest-looking shortcut.

Care about the product outcome (fast, correct, polished, maintainable), not the task checkbox.

## Step by step (always)

1. **Understand** — Read the goal, domain spec, references, and (if debugging) logs/metrics/profiles. List what is unknown. Ask or research — never silently assume.
2. **Sanity-check** — Flag scope conflicts, impossible tradeoffs, and implicit quality bars (performance, polish, UX). Propose a sane cut before building.
3. **Research** — How does this codebase, prior art, and shipped products solve this? Prefer boring proven patterns over clever one-offs.
4. **Plan** — State expected outcome, side effects, and what "done" means. Hypothesis → verify. Not act → hope.
5. **Execute** — When a visual/API/product contract exists, implement plumbing — do not invent a second look or API. Check signals as you go; don't wait until the end to discover the approach is wrong.
6. **Verify** — Build, test, profile, or visually check against acceptance criteria. Todos complete ≠ done. No half-migrations or dual systems without explicit deferral.
7. **Stay clean** — No scratch litter in the repo. Rotate artifacts (current/previous); consolidate knowledge. You read your own mess later.
8. **Evaluate and adapt** — After every failure or surprise: pause, name what happened, state what you learned, update the hypothesis. If evidence shows the approach is wrong or progress has stalled, pivot — list alternatives and pick one. Abandon by evidence, not sunk cost.

## Hard stops (never)

| Never | Do instead |
|---|---|
| Mask symptoms (timeouts, pixel tweaks) | Find and fix root cause; state tradeoff if truly intractable |
| Fill gaps silently | Ask or investigate |
| Clever hacks (megashaders, procedural UI) when industry uses simple primitives | Use the proven pattern |
| Mark done without verification | Run the check that matches the goal |
| Keep grinding without learning from failures | Pause, state what failed and why, then retry deliberately or pivot |
| Accumulate stale docs, captures, scratch files | Overwrite, rotate, consolidate |

## Cross-references

- `clarify-requirements` — scope alignment before implementation
- `code` — code-specific quality; references this skill for process discipline
- `code-review` — systemic pass uses [principle catalog](../learn/references/principle-catalog.md)
