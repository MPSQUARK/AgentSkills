---
name: code-review
description: >-
  Mandatory self-review after writing code; deep line-by-line review on /code-review.
  Use when: finishing any code change, before submitting work, user asks for code review,
  or reviewing own work for duplication, correctness, and forward-looking improvements.
disable-model-invocation: true
---

# Code Review

Stricter on your own work than others'. Check every line. "Works" is insufficient — must be clean, reusable, defensible.

## When

| Mode | Trigger |
|---|---|
| **Self-review gate** | After any code write/modify, before marking task done (`code` skill requires this) |
| **Deep review** | `/code-review` or user asks for thorough review |

## Flow

`Context → line-by-line → systemic pass → 2–4 moves ahead → verdict`

Issues found → fix → **re-review the fix** (don't assume patch is clean).

## Line-by-line (every changed line)

- Correct for execution niche? Domain logic including non-happy-path (empty, zero, bounds, errors)?
- Invariants documented where needed and upheld elsewhere?
- Performance reasonable for context — not premature optimization, not obvious hot-path mistakes?
- Right abstraction level? Duplicated in diff, file, or codebase?
- Belongs in this file/class or warrant extraction?
- Refactor: behavior **equivalent** (or intentional + tested)?

## Public surface audit (library/module changes)

- Minimize visibility — prefer internal/private
- Exported API matches siblings in shape and error model
- Defaults safe against misuse?

## Systemic pass

Walk [principle catalog](../learn/references/principle-catalog.md) — any violation?

- Reused existing patterns? New shared mutable state or hidden coupling?
- File organization smells? Would a domain specialist approve?
- Similar issues in adjacent code glossed over?

## Similar-code sweep (mandatory)

Grep parallel implementations (same structure, pattern, concern). If session corrected pattern P, verify new code doesn't reintroduce P.

Output: `Similar code checked: [patterns] — aligned / fixed / N/A`

## Adjacency review

Per changed file, skim 1–3 callers/siblings/prior art in the module for the same issue class. Don't limit to diff hunk.

## Think 2–4 moves ahead

- Next refactor this design forces?
- Second feature that will undo or duplicate this?
- Papering over structural debt?
- Record 1–3 notes: fix now vs acceptable debt

## Output

**Self-review gate**: brief verdict — pass, or issues found + fixes applied.

**Deep review**: table sorted by severity — Severity | Location (file:line) | Finding | Suggested fix

## Strictness

Do not gloss over similar issues. Incomplete error paths, partial refactors, and copy-paste drift are blocking defects, not nitpicks.
