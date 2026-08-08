# Gauntlet Loop — Reference

Supplementary material for the `gauntlet-loop` skill. Read when you need templates, examples, critic prompts, or failure-mode guidance.

---

## Canonical Example: Claude of Duty

Matt Shumer's [Claude of Duty prompt](https://github.com/mshumer/Claude-of-Duty/blob/main/prompt.md) is the reference implementation of the Gauntlet Loop pattern. The entire prompt:

```
I want you to build a first-person shooter at the level of the most recent Call of Duty games. It should be utterly perfect, visually beautiful, with every single thing done at AAA quality—from textures to physics to anything you could think of.

Fan out sub-agents and have sub-agents tackle each one individually so that the game is utterly perfect. You should /loop on each item and have a separate sub-agent check it visually to ensure it looks triple A. That separate sub-agent should be a really harsh critic, and if it doesn't look triple A, it should keep going.

Don't stop until each sub-agent is utterly wowed with the quality when compared with the actual Call of Duty game. It should literally compare them side by side blind and say which one looks better. Do this in ThreeJS. /loop until it's utterly perfect. Fan out sub-agents and ultracode.
```

### Why each paragraph works

| Paragraph | What it does |
|-----------|--------------|
| **1 — Destination** | Names the artifact (FPS) and a concrete quality bar (recent Call of Duty, AAA). Does not prescribe architecture, renderer details, or subsystem list. |
| **2 — Decomposition + critics** | Permission to fan out sub-agents per piece; `/loop` per item; separate harsh critic with fresh context; critic inspects visually. |
| **3 — Comparison + persistence** | Blind side-by-side against real reference; names technology (ThreeJS) only as constraint; loops until bar wins; "ultracode" = high reasoning effort in Claude Code. |

**Important honesty note:** Shumer reported that blind comparisons still preferred real Call of Duty. The bar is a **compass**, not a guaranteed finish line. The value is sustained improvement, not claiming parity.

---

## Copy-Paste Templates

### Template 1: Minimal Gauntlet prompt

Use inside the orchestration prompt section of the gauntlet spec:

```
I want you to create <DELIVERABLE> that achieves <OBJECTIVE> at the quality
level of <CONCRETE REFERENCE OR MEASURABLE BENCHMARK>.

Choose the approach. Break the work into the smallest important parts that can
be improved and judged independently. Fan out builders only where the work is
genuinely independent. Give every important part a separate, harsh critic with
fresh context.

Each critic must inspect the real output—not the builder's summary—and compare
it directly with the reference or metric, using a blind A/B comparison where
possible. If our result loses, identify the largest meaningful gap, return it
to the builder, and run another round.

Keep looping until the output meets <SUCCESS CONDITION>, improvements no longer
justify another round, or one of these boundaries fires: <TIME / COST / ATTEMPT /
PERMISSION / SAFETY BOUNDARIES>. Escalate blockers that require human judgment.

Finish with one fresh integration critic that checks the complete artifact for
consistency, correctness, and fit with the original objective.

For coding, use <PROGRAMMING LANGUAGE / FRAMEWORK>. Do not deploy, spend money,
use credentials, contact people, or make irreversible changes without explicit
approval.
```

### Template 2: Bounded loop card

Use when reliability and cost matter more than dramatic language:

```
OBJECTIVE
<Write the exact outcome that should become true.>

INPUTS AND STATE
Use: <files, sources, tools, project, progress log at /memories/session/gauntlet-progress.md>.
Record after every round: what changed, evidence, score, failed approach,
next action, and remaining budget.

METRIC / VERIFIER
Success requires all of the following:
- <objective test, benchmark, or factual check>
- <quality rubric or reference comparison>
- <integration, accessibility, safety, or editorial check>

PROCESS
1. Inspect the current state.
2. Choose the highest-impact unmet criterion.
3. Make one coherent improvement.
4. Run the real verifier.
5. If it fails, feed the evidence into a changed strategy and repeat.
6. If it passes, run a fresh independent final review.

BOUNDARIES
Allowed actions: <read, draft, edit, test, render>.
Forbidden without approval: <deploy, delete, purchase, publish, message, secrets>.
Stop and report when: success passes; <N> attempts finish; <TIME/COST> is
reached; the same blocker repeats; or uncertainty exceeds <THRESHOLD>.
```

---

## Critic Prompt Skeleton

Use when launching a critic `Task` subagent. **Do not include builder reasoning or chat history.**

```
You are a harsh, independent critic. You have fresh context. You did NOT build this artifact.

## Objective (snippet)
[One paragraph from gauntlet spec]

## Quality bar
Reference: [name, links, files]
Comparison method: [blind A/B, test suite, rubric]

## Metrics — all must pass for PASS
1. [verifier 1]
2. [verifier 2]
...

## Artifact to inspect
- Paths: [files, URLs, screenshot paths]
- How to verify: [commands to run, browser steps, read files]

## Rules
- Inspect the REAL output. Run tests. View screenshots. Read code. Do not trust summaries.
- Compare directly against the reference where possible.
- If blind A/B applies, state which wins and why with evidence.
- Name exactly ONE largest meaningful gap if FAIL.
- Provide a concrete fix target the builder can act on.

## Output format
VERDICT: PASS | FAIL | BLOCKED
LARGEST_GAP: [one sentence, or "none" if PASS]
FIX_TARGET: [concrete correction, or "none" if PASS]
EVIDENCE: [paths, command output, screenshot refs]
```

### Integration critic variant

Same skeleton, but scope is the **complete artifact** across all pieces:

- Check seams between subsystems
- End-to-end objective still met
- No regressions in previously passed pieces
- Visual/behavioral consistency

---

## Builder Prompt Skeleton

Use when launching a builder `Task` subagent:

```
You are a builder for piece [ID]: [name].

## Scope
[What this piece must deliver — independently judgeable]

## Skills to follow
- Read and follow the `code` skill for all implementation.
- [If UI: Read and follow the `frontend-design` skill.]

## Quality bar (for awareness — you are NOT the critic)
Reference: [snippet]
Your work will be judged by a separate critic inspecting real output.

## Constraints
- Stack: [from spec]
- Workspace: [allowed dirs]
- Out of scope: [from spec]
- Do not: [forbidden actions]

## If this is a revision round
Largest gap from last critic: [gap]
Fix target: [target]
Failed approaches (do not repeat): [list from progress log]

## Deliverable
Produce observable artifact: [code files, runnable demo, screenshots].
Report: files changed, how to verify, evidence paths.
```

---

## Non-Game Examples

### Book / long-form writing

| Element | Example |
|---------|---------|
| Objective | 45,000-word practical guide fulfilling approved chapter outline |
| Bar | Approved outline + style samples + fact sheet |
| Metrics | Every claim traces to research; chapter rubrics pass; fresh editor finds no blocking issue |
| Boundaries | Max 5 critic rounds per chapter; no invented sources; human approval before "final" |
| Decomposition | Research coverage, argument, structure, examples, prose, fact-check, continuity |

### Marketing / product website

| Element | Example |
|---------|---------|
| Objective | Responsive product page explaining offer; completes existing signup journey |
| Bar | Approved reference sites (screenshots) |
| Metrics | No a11y violations; no overflow at 360px; performance budget; form behavior preserved |
| Boundaries | Local only; no deploy; stop after budget or 2 rounds with no measurable gain |
| Decomposition | Hierarchy, visual system, responsive behavior, copy, a11y, performance, E2E signup |

### Software feature

| Element | Example |
|---------|---------|
| Objective | Implement scoped feature in [repo] without changing unrelated behavior |
| Bar | Issue acceptance criteria + reference module in codebase |
| Metrics | Acceptance tests, unit tests, lint, static analysis, security checks, spec review |
| Boundaries | No prod changes, schema deletion, new paid deps; stop on missing requirements |
| Decomposition | Exploration, implementation, tests, security, usability, spec verification — coupled code keeps one owner |

---

## Failure Modes Checklist

Before and during a run, watch for:

| Failure mode | Symptom | Fix |
|--------------|---------|-----|
| Subjective goal | "Perfect" with no inspectable evidence | Add concrete reference, tests, or rubric |
| Builder-as-judge | Same agent builds and grades | Separate critic `Task` with fresh context |
| Gameable metric | Passes one score, fails real objective | Multiple guardrail verifiers |
| No budget boundary | Runs until user kills it | Declare time/cost/round limits in spec |
| Spinning | Same failure, same approach | Log failed approaches; escalate or change strategy |
| Context rot | Long chat, lost state | Maintain `gauntlet-progress.md`; compact updates |
| Agent collision | Parallel edits on coupled systems | Sequential owner for coupled pieces |
| Self-reported progress | "Looks good" without evidence | Critics must run real verifiers |
| Permissions too broad | Deploy/delete without gate | Forbidden list in boundaries |
| Skipped integration | Pieces pass locally, whole fails | Mandatory integration critic |
| Over-prescription | Prompt lists architecture | Keep orchestration prompt short; agent chooses route |

### When NOT to use a Gauntlet Loop

- Simple, clearly-scoped task (rename, typo, single bug fix) → use direct implementation or `clarify-requirements`
- No inspectable quality bar exists and cannot be created
- No agent harness (tools, subagents, file access, test execution)
- One-shot answer is sufficient

---

## Decomposition Heuristics

**Good pieces (independently judgeable):**
- Weapon rendering vs enemy AI vs HUD layout
- API endpoint vs its unit tests vs its OpenAPI doc
- Chapter draft vs fact-check of that chapter

**Keep sequential (coupled):**
- Scene lighting + material system + post-processing (visual coherence)
- Auth middleware + session store + login UI (tight dependency)
- Database schema + migration + all queries touching that schema

**Claude of Duty lesson:** Broad fan-out on coupled visual systems performed worse than sequential ownership with focused critics.

---

## Progress Log Discipline

After **every** builder or critic round, append to `/memories/session/gauntlet-progress.md`:

1. Timestamp, piece ID, round number
2. What changed or what was inspected
3. Evidence paths (not descriptions)
4. Verdict and largest gap
5. Failed approaches to avoid repeating
6. Remaining budget

Long chat histories rot. The progress log is the durable memory across subagent context windows.
