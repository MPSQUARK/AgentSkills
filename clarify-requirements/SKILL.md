---
name: clarify-requirements
description: 'ALWAYS invoke at the start of the planning phase before any plan or implementation begins. Auto-suggest for: complex features, architectural changes, new pages or services, cross-cutting concerns, anything spanning multiple files or systems, any request where intent is ambiguous or scope is unclear. Runs a structured alignment interview — acknowledged what is asked, investigates the codebase proportionally, asks all clarifying questions upfront, iterates until fully resolved, then produces a written specification in /memories/session/plan.md before any implementation proceeds.'
---

# clarify-requirements

Instructions for the AI agent...

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

Decide how much codebase investigation is warranted based on the scale and complexity of the change:

- **Simple / localized change** (e.g., rename a label, fix a typo, change a colour): Skip or minimise this step. Do not slow the user down by surfacing irrelevant context.
- **Moderate change** (e.g., add a new field, tweak a flow): Check directly related files only. Briefly note any existing patterns or constraints relevant to the questions below.
- **Significant / architectural change** (e.g., new feature, new page, new service, cross-cutting concern): Investigate thoroughly. Before listing questions, provide a concise "What I found in the codebase" section summarising:
  - Relevant existing files, patterns, and naming conventions
  - Current implementation of anything this change touches
  - Anything in the codebase that constrains or informs the design

Surface only findings that directly affect the questions or decisions ahead.

---

### Step 3 — Ask All Clarifying Questions Upfront

Present **all** questions as a single numbered list. Do not ask one at a time — give the full list in one response so the user can answer everything efficiently.

Cover every area relevant to this specific request. Skip areas that are clearly not applicable (e.g., skip UI/UX questions for a pure backend task). Always consider:

1. **Functional requirements** — What exactly must this feature/change do? Inputs, outputs, and behaviours?
2. **UI/UX expectations** *(if applicable)* — How should it look and feel? Layouts, interactions, feedback, animations, states (loading, empty, error)?
3. **Out-of-scope boundaries** — What should explicitly NOT be built or changed? What are the edges of this work?
4. **Technical approach** — Constraints on architecture, patterns, libraries, or frameworks? Should it follow an existing pattern in the codebase?
5. **Edge cases and error handling** — What happens in failure scenarios, unexpected inputs, or boundary conditions?
6. **Acceptance criteria** — How will we know this is done? What does success look like?
7. **Any vague points detected** — Flag anything ambiguous, underspecified, or open to multiple interpretations.

When the request involves non-trivial design or implementation decisions, also consider:

8. **Design decisions & patterns** — Does this follow SRP? Which pattern fits best (MVVM, service layer, repository, etc.)? Should this be a new service/component or extend an existing one?
9. **Code reuse** — Are there existing components, helpers, or services that should be leveraged? Does this change create a proactive refactoring opportunity worth raising?
10. **Consequences of design choices** — What side effects does the proposed approach have on other components? What becomes harder to change later?
11. **Performance concerns** — Any risk of N+1 queries, UI-thread blocking, memory leaks, or excessive allocation?
12. **Gaps, ambiguities, and edge cases** — What happens in error or boundary conditions? Are there behaviours left underspecified that could cause misalignment during implementation?

> If a question can be answered with confidence by checking the codebase (Step 2), check it first and do not ask the user.

---

### Step 4 — Iterate Until Everything Is Resolved

After the user answers, review their responses:

- If new ambiguities have surfaced, present a follow-up numbered list with only the remaining or new questions.
- If everything is clear, proceed to Step 5.
- Repeat as many times as needed — there is no limit.

Do NOT proceed to Step 5 with any unresolved questions, unstated assumptions, or vague areas that could lead to misalignment.

---

### Step 5 — Produce the Specification

Only when **all** questions are fully resolved, write a structured specification to `/memories/session/plan.md`:

```
# Feature: [Name]

## Summary
[2–4 sentence description of what is being built and why.]

## Functional Requirements
[Numbered list of concrete, testable requirements.]

## UI/UX Expectations
[Description of look, feel, interactions, and states — or "N/A".]

## Out of Scope
[Explicit list of what will NOT be built in this iteration.]

## Technical Approach
[Architecture decisions, patterns to follow, libraries to use, constraints.]

## Edge Cases & Error Handling
[How the system behaves under failure or boundary conditions.]

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
- If the user tries to skip this process, acknowledge their preference, note the risks, and proceed only if they explicitly confirm they want to bypass alignment.
