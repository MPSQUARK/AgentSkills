---
name: clarify-requirements
description: 'ALWAYS invoke at the start of the planning phase before any plan or implementation begins. Auto-suggest for: complex features, architectural changes, new pages or services, cross-cutting concerns, anything spanning multiple files or systems, any request where intent is ambiguous or scope is unclear. Runs a structured alignment interview — acknowledges what is asked, investigates the codebase proportionally, prefers AskQuestion multiple-choice rounds with multi-layer iteration until gaps close, then produces a written specification in /memories/session/plan.md before any implementation proceeds.'
---

# clarify-requirements

Structured alignment for a **single feature or change** before any plan or implementation. Produces a session-scoped specification; for full domain/system documentation, use `design-document-discovery` first.

## Relationship to design-document-discovery

| Skill | Scope | Output | When |
|---|---|---|---|
| `design-document-discovery` | Full domain / system | Domain spec in discovered docs path | Exhaustive vision-vs-code alignment |
| `clarify-requirements` | Single feature / change | `/memories/session/plan.md` | Before implementing one task |

**Order of use:** `design-document-discovery` establishes domain truth; `clarify-requirements` scopes individual features against it. Each feature plan should link to the relevant domain spec sections.

Teams without Cursor memories may use an equivalent session plan path if the user specifies (e.g. `docs/plans/{feature}.md`).

## When to Use

- **Always** at the start of the planning phase — before any plan, design, or code is produced
- **Auto-suggest** this skill whenever the request involves:
  - A new feature, page, or service
  - An architectural or cross-cutting change
  - Anything that touches multiple files or systems
  - A request where intent, scope, or technical approach is ambiguous
  - Moderate-to-complex changes where assumptions could lead to wasted work

> For simple, clearly-scoped tasks (e.g., rename a label, fix a typo), this workflow can be abbreviated — but never fully skipped.

---

## Procedure

### Step 1 — Acknowledge and Decompose the Request

Read the feature/task description carefully. Briefly (2–4 sentences) acknowledge what was understood, then identify:

- What is clearly stated and unambiguous
- What is implied but not confirmed
- What is vague, missing, or open to multiple interpretations

---

### Step 2 — Proportional Codebase Check

**Domain spec first:** If a domain spec exists (from `design-document-discovery` or repo doc discovery — e.g. `docs/specs/`, `Documentation/`, `ARCHITECTURE*`), read the relevant sections before asking questions. Do not re-ask what the domain spec already answers; reference it in the plan instead.

Decide how much codebase investigation is warranted based on the scale and complexity of the change:

- **Simple / localized change** (e.g., rename a label, fix a typo, change a colour): Skip or minimise this step. Do not slow the user down by surfacing irrelevant context.
- **Moderate change** (e.g., add a new field, tweak a flow): Check directly related files only. Briefly note any existing patterns or constraints relevant to the questions below.
- **Significant / architectural change** (e.g., new feature, new page, new service, cross-cutting concern): Investigate thoroughly. Before listing questions, provide a concise "What I found in the codebase" section summarising:
  - Relevant existing files, patterns, and naming conventions
  - Current implementation of anything this change touches
  - Anything in the codebase that constrains or informs the design
  - Relevant domain spec sections already covering this area

When the change touches an **unfamiliar or performance-critical niche**, skim existing implementations **in that same niche** in the codebase before listing questions.

Surface only findings that directly affect the questions or decisions ahead.

---

### Step 3 — Clarifying Questions (AskQuestion preferred)

When `AskQuestion` is available, use it for clarifying questions — **prefer over chat numbered lists**.

- **~5 questions per round**, grouped by theme (functional, scope, technical, edge cases, etc.)
- Offer **concrete multiple-choice options** inferred from codebase and context — not bare Yes/No
- Every question includes **"Other / I'll explain in chat"**
- Use `allow_multiple: true` for checklist-style scope questions
- Do not re-ask what Step 2 or an existing repo spec already resolved

For trivial tasks (rename, typo), abbreviated mode: 1–2 `AskQuestion` calls or brief chat is OK.

Use the following as an **agent-only coverage guide** when building rounds — do not present this list to the user. Skip areas that are clearly not applicable (e.g., skip UI/UX for a pure backend task). Always consider:

1. **Functional requirements** — What exactly must this feature/change do? Inputs, outputs, and behaviours?
2. **UI/UX expectations** *(if applicable)* — How should it look and feel? Layouts, interactions, feedback, animations, states (loading, empty, error)?
3. **Out-of-scope boundaries** — What should explicitly NOT be built or changed? What are the edges of this work?
4. **Technical approach** — Constraints on architecture, patterns, libraries, or frameworks? Should it follow an existing pattern in the codebase?
5. **Edge cases and failure modes** — Empty, zero, overflow, cancellation, errors — or explicitly N/A with reason
6. **Quality bar** — Correctness approach, execution context (e.g. multithreaded), invariants, error model, performance constraints
7. **Reuse expectations** — Existing patterns/APIs to leverage; forbidden duplication
8. **Acceptance criteria** — How will we know this is done? What does success look like?
9. **Any vague points detected** — Flag anything ambiguous, underspecified, or open to multiple interpretations.

When the request involves non-trivial design or implementation decisions, also consider:

10. **Design decisions & patterns** — Does this follow SRP? Which pattern fits best (MVVM, service layer, repository, etc.)? Should this be a new service/component or extend an existing one?
11. **Consequences of design choices** — What side effects does the proposed approach have on other components? What becomes harder to change later?
12. **Performance concerns** — Any risk of N+1 queries, UI-thread blocking, memory leaks, or excessive allocation?

When the change touches an **unfamiliar or performance-critical niche**, also consider:

13. **Execution context** — Where does this code run (CPU hot path, background worker, device/accelerator, I/O boundary)? What constraints apply (latency, throughput, memory, parallelism)?
14. **Unit of work** — What is the natural grain of one operation in this context? What should explicitly *not* be nested or looped inside it?
15. **Minimal structure** — What is the smallest design that meets requirements? What abstractions are being rejected as unnecessary?
16. **Project domain docs** — Does this repo have a spec or guide for this niche? (Resolve per project — read it before asking the user to repeat it.)
17. **Verification boundary** — Where do automated tests belong vs user manual or scratch areas?

> If a question can be answered with confidence by checking the codebase (Step 2), check it first and do not ask the user.

---

### Step 4 — Iterate Until Everything Is Resolved

After each `AskQuestion` round (or chat answer), review responses:

1. **Synthesize** — brief bullets of decisions taken from answers
2. **Gap check** — did answers introduce new ambiguities or conflicts? Cross-check the Step 3 concern checklist
3. **Follow-up** — only unresolved or newly surfaced gaps; reference prior answers in question text and option labels
4. **Confirm** — before Step 5, confirm nothing remains (via `AskQuestion` when available)

Repeat until no new gaps from the gap check **and** the user confirms ready. Then proceed to Step 5.

Do NOT proceed to Step 5 with any unresolved questions, unstated assumptions, or vague areas that could lead to misalignment.

---

### Step 5 — Produce the Specification

Only when **all** questions are fully resolved, write a structured specification to `/memories/session/plan.md`:

```
# Feature: [Name]

## Summary
[2–4 sentence description of what is being built and why.]

## Domain Spec Reference
- Spec: [path/to/domain-spec.md] (or "N/A — no domain spec yet")
- Relevant sections: [section numbers/names]

## Functional Requirements
[Numbered list of concrete, testable requirements.]

## UI/UX Expectations
[Description of look, feel, interactions, and states — or "N/A".]

## Out of Scope
[Explicit list of what will NOT be built in this iteration.]

## Technical Approach
[Architecture decisions, patterns to follow, libraries to use, constraints.]

## Quality Bar
[Correctness approach, execution context, invariants/contracts, error model, performance constraints.]

## Reuse Expectations
[Patterns/APIs/files to reuse; forbidden duplication; discovery from Step 2.]

## Edge Cases & Failure Modes
[Explicit list or "inherits from sibling X". Mark N/A only with reason.]

## Acceptance Criteria
[Numbered list of conditions that must be true for this to be considered done.]

## Open Questions
[Any remaining unknowns to be decided during implementation — ideally empty.]
```

Then present the specification to the user in chat and **wait for explicit approval** before any planning or implementation begins.

---

## Strict Enforcement Rules

- **NEVER** write code, suggest implementation details, or draft a plan until Step 5 is complete and the user has explicitly approved.
- **NEVER** make silent assumptions. If an assumption is necessary, state it explicitly and ask for confirmation.
- **NEVER** skip this workflow because the request seems simple. Even simple requests benefit from explicit scope confirmation.
- **Prefer `AskQuestion` with concrete options** over numbered chat lists when the tool is available.
- **NEVER** ask follow-ups that ignore prior answers.
- **NEVER** proceed to Step 5 without synthesizing all rounds into the spec.
- If the user tries to skip this process, acknowledge their preference, note the risks, and proceed only if they explicitly confirm they want to bypass alignment.
