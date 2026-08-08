# Gauntlet Loop: [Name]

## Summary

[2–4 sentence description of the artifact, quality ambition, and why a gauntlet loop is appropriate.]

## Domain Spec Reference

- Spec: [path/to/domain-spec.md] (or "N/A — no domain spec yet")
- Relevant sections: [section numbers/names]

## Objective

[Exact outcome that must become true. Destination, not route.]

## Quality Bar

- **Reference:** [name, links, files, screenshots]
- **Comparison method:** [blind A/B, test suite, rubric, benchmark, side-by-side render]
- **What critics inspect:** [pixels, test output, logs, citations — not builder summaries]

## Metrics / Verifiers

Success requires all of the following:

1. [Objective test, benchmark, or factual check]
2. [Quality rubric or reference comparison]
3. [Integration, accessibility, safety, or editorial check]
4. [Additional verifier]
5. [Additional verifier]

## Boundaries

### Allowed actions

- [read, draft, edit, test, render, spawn subagents, ...]

### Forbidden without approval

- [deploy, delete, purchase, publish, message, secrets, ...]

### Stop when

- [Success condition passes]
- [Time budget: e.g. 8 hours]
- [Cost/token ceiling if relevant]
- [Max rounds per piece: e.g. 5]
- [Same blocker repeats N times]
- [User stops the run]

### Workspace scope

- [Which directories may change]

## Technology and Constraints

- **Stack:** [language / framework / or "agent chooses"]
- **Patterns to follow:** [existing repo conventions]
- **Out of scope:** [explicit exclusions]

## Decomposition Plan

Draft — lead agent may refine after approval.

| ID | Piece | Independent? | Coupled with | Builder skills | Critic type |
|----|-------|--------------|--------------|----------------|-------------|
| P1 | [name] | yes/no | [IDs or N/A] | code, frontend-design | visual / test / rubric |
| P2 | | | | | |

## Piece Registry

| ID | Piece | Owner type | Critic type | Status | Rounds |
|----|-------|------------|-------------|--------|--------|
| P1 | | builder | | pending | 0 |

Status values: `pending` | `building` | `criticizing` | `passed` | `blocked` | `skipped`

## Orchestration Prompt

[2–4 short paragraphs in Matt Shumer style. High bar, minimal prescription, fan-out builders, fresh critics, /loop per piece, integration critic at end. Do not prescribe architecture or fixed round counts.]

## Progress Log Path

`/memories/session/gauntlet-progress.md`

## Open Questions

[Any remaining unknowns — ideally empty before execution.]
