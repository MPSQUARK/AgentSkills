---
name: learn
description: 'Capture durable knowledge as agent skills. Use when: architecture decisions, conventions, debugging findings, repeated explanations, user corrections, confirmed patterns. Always generalize to principles — never incident-only facts. DO NOT write any file without explicit user approval.'
---

# learn — Long-Term Knowledge

Curated skills for future agents with no prior context. `/memories/` = transient session state; learn outputs = permanent, discoverable knowledge.

## Triggers

- **Manual**: `/learn`
- **Mandatory on user correction** — run correction pipeline (below) before moving on
- **Proactive** (approval still required): non-obvious quirk, explained concept, confirmed pattern, repeated topic, debugging finding

## Correction → Learn pipeline

On any user correction:

1. **Name the principle** in chat — which general rule was violated?
2. **Check** [principle-catalog](./references/principle-catalog.md) and existing skills — already covered?
3. **Sweep** codebase for the same anti-pattern; fix or flag
4. **Propose** learn update — principle first, incident as optional example only

## Procedure

### 1 — Identify

Articulate: principle (not incident), why it matters, project vs global scope.

### 1.5 — Principle extraction (mandatory)

Before lookup or drafting:

- State the **general principle first**; incident is optional illustration only
- **Reject** proposals that restate the mistake or name a symbol without a transferable rule
- **Test**: would this guide an agent on a *similar but not identical* problem?
- **Tag** a [catalog](./references/principle-catalog.md) category
- **Persist principle only** — incident details stay in chat/approval preview, never in skill files

| Reject if the draft… | Capture instead… |
|---|---|
| Names a symbol, file, method, or one-off fix | Category + transferable rule (role names OK: "scratch buffer", "accumulator") |
| Only says what not to do | What to do, when it applies, how to discover the right pattern |
| Needs the original bug story to make sense | Stands alone for a cold-start agent |
| Paraphrases one correction narrowly | Generalizes to the whole class of similar work |

**Extraction ladder** (run mentally on every draft): incident → category → principle. Only the bottom row gets written.

### 2 — Lookup first

Enumerate skills in `~/.cursor/skills/`, `~/.agents/skills/`, `~/.copilot/skills/`, and project `.cursor/skills/`, `.agents/skills/`, `.github/skills/`. Read `description` fields. **Update existing** skill if domain matches; create new only when none fits.

### 3 — Scope

| Scope | Path | When |
|---|---|---|
| Global (Cursor) | `~/.cursor/skills/<domain>/` or `~/.agents/skills/<domain>/` | Cross-project |
| Global (Copilot) | `~/.copilot/skills/<domain>/` | Cross-project |
| Project | `<workspace>/.cursor/skills/<domain>/` etc. | Codebase-specific |

Prefer **project** when in doubt.

### 4 — New or update?

**Update** (preferred): merge into existing principle bullet — never add a parallel bullet restating the same idea. Consolidate overlaps.

**Create new**: only when no domain covers it. Folder name = broad domain (`dotnet-patterns`), not a fact (`reconnection-fix`).

### 5 — Draft

- Updates: exact lines and location
- New: full `SKILL.md` from template below
- Structure: **principle + optional example** — forbid example-only learnings
- Must be generalizable on first read and actionable without the original bug context

### 6 — Approval (mandatory)

`AskQuestion`: action, path, content preview, Approve/Reject + edits. Never write without explicit approval.

### 7 — Write

Create or update on approval. Reference files at `references/<subtopic>.md`.

### 8 — Gap scan

Propose ≤2–3 more captures from the session; apply principle extraction to each.

## New skill template

```markdown
---
name: <domain>
description: 'Use when: <triggers>. Covers: <topics>.'
---
# <Domain>
## Overview
## Key Patterns
## Sub-topics
## Gotchas & Edge Cases
## Cross-references
```

**Content rules**: specific and actionable; gotchas = failure mode + correct approach; cross-refs name real skills.

## Naming

Lowercase-hyphenated; domain/framework level. Examples: `csharp-async`, `projectname-architecture`. Avoid: `misc`, `my-notes`, `todo`.

## Memory boundary

| System | Lifetime |
|---|---|
| `/memories/session/` | Current session |
| `/memories/repo/` | Informal persistent |
| Learn outputs | Permanent, indexed by description |

Reusable by a cold-start agent → learn it, don't just memory-note it.
