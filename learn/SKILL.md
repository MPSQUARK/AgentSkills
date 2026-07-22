---
name: learn
description: 'Capture, update, and organize durable knowledge as Copilot skills for reuse across sessions. Use when: recording project architecture decisions, saving framework quirks or non-obvious behaviors, preserving debugging findings, capturing coding conventions and patterns, reducing repeated explanations, building context for future agents. Also invoke when: a mistake was corrected by the user, a concept had to be explained more than once, a non-obvious workaround was discovered, or an established pattern was confirmed. DO NOT write any file without explicit user approval.'
---

# learn — Copilot Long-Term Knowledge System

## Purpose

This skill captures curated, durable knowledge as Copilot skill files so future agents — and agents whose context window has been compacted — can load relevant knowledge on demand without requiring the user to repeat themselves. It is the long-term memory layer of the system.

**Relationship to `/memories/`**:
- `/memories/` → transient working notes, session state, in-progress task context. Session-scoped or short-lived.
- Learn skill outputs → durable, discoverable knowledge intended for future agents with no prior context.

When something moves from a working note to an established fact worth reusing across sessions → Learn it.

---

## Trigger Conditions

**Manual**: User invokes `/learn` explicitly to capture something specific.

**Proactive (agent-initiated, approval still required)**: Propose capturing knowledge when any of the following occurs during a session:
- The user corrected the agent's assumption or approach
- The agent had to reason through something non-obvious (framework quirk, undocumented behavior, workaround)
- The user explained a project-specific concept or architecture decision
- A pattern, convention, or constraint was established or confirmed
- The same topic has come up more than once across the conversation
- A debugging finding revealed something worth remembering

> **Important**: Proactive proposals must still go through the approval workflow. Never write anything without explicit approval.

---

## Procedure

### Step 1 — Identify What to Capture

Clearly articulate:
- What was learned (the fact, pattern, quirk, decision, or finding)
- Why it is worth capturing (what problem it solves or context gap it fills)
- Whether it is specific to the current project or broadly applicable

### Step 2 — Lookup First (Always)

Before proposing anything, check existing skills in both locations:

**Global skills** (applies across all projects):
```
~/.copilot/skills/
```

**Project skills** (for current workspace):
```
<workspace-root>\.github\skills\
```

Use `list_dir` to enumerate folders, then read the `description` field of each `SKILL.md` to find candidates.

**Rule**: If a relevant domain skill already exists → propose updating it (add to body, add a reference file, expand a section). Only propose creating a new skill folder if no existing domain covers the topic.

### Step 3 — Determine Scope

| Scope | Location | Use When |
|-------|----------|----------|
| **Global** | `~/.copilot/skills/<domain>/` | Framework knowledge, language patterns, tool usage, debugging techniques, general architecture concepts — applicable across multiple projects |
| **Project** | `<workspace-root>\.github\skills\<domain>\` | Current project architecture, data models, service patterns, coding conventions, flows unique to this codebase |

If in doubt, prefer **project scope** — it avoids polluting global knowledge with context that only makes sense for one project.

### Step 4 — Decide: New Skill or Update Existing?

**Update existing** (preferred): Add knowledge to an existing domain skill. Options:
- Add bullet points to an existing section in `SKILL.md`
- Add a new reference file at `references/<subtopic>.md` and link it from `SKILL.md`
- Expand the `Gotchas & Edge Cases` section with a new finding
- Add a cross-reference to a related skill

**Create new skill**: Only when no existing domain covers the topic. The new folder name must be lowercase-hyphenated and represent the domain broadly (not the specific fact). See naming conventions below.

### Step 5 — Draft the Content

Prepare the exact content to be written:
- For updates: the specific lines to add and exactly where
- For new skills: the complete `SKILL.md` using the template below
- Content must be specific, actionable, and non-vague — no filler, no hand-waving

### Step 6 — Ask for Approval (Mandatory)

**Never write any file without explicit user approval.**

Use `vscode_askQuestions` to present:
1. **Action**: "Create new skill `<name>`" or "Update existing skill `<name>`"
2. **Location**: Full file path
3. **Content Preview**: The exact content to be written (full text for new files; exact diff/addition for updates)
4. Provide at minimum: Approve / Reject options, plus a free-text field for modifications

Only proceed after the user explicitly approves. If they provide edits in the free-text field, incorporate the changes and confirm before writing.

### Step 7 — Write the File

On approval, create or update the file using the appropriate tool. For new skills, create the folder and `SKILL.md`. For reference sub-files, create at `references/<subtopic>.md` within the domain skill folder.

### Step 8 — Proactive Gap Scan

After completing the learn action, briefly reflect:
- Are there other things from this session that aren't captured and should be?
- Is anything in the current task context that would reduce future repeated explanations?

If yes, propose them (following the same approval workflow). Do not flood the user — propose at most 2-3 items at a time.

---

## Generated Skill Template

Use this template for all new knowledge skills created by this workflow:

```markdown
---
name: <domain-name>
description: 'Use when: <specific trigger phrases — what situation warrants loading this skill>. Covers: <list of main topics this skill addresses>.'
---

# <Domain Name>

## Overview
<2–4 sentences describing what this domain covers, why it matters, and when it is relevant.>

## Key Patterns
- <Core fact, rule, or pattern #1>
- <Core fact, rule, or pattern #2>
- ...

## Sub-topics
- [<Sub-topic Title>](./references/<subtopic>.md) — <one-line description of what this reference covers>

## Gotchas & Edge Cases
- <Non-obvious issue, unexpected behavior, or common mistake #1>
- <Non-obvious issue, unexpected behavior, or common mistake #2>

## Cross-references
- Related: `<other-domain-skill-name>` — <why it is related and when to load both>
```

**Rules for content quality**:
- Every bullet must be specific and actionable — never write "ensure proper error handling" or similar vague guidance
- Include concrete examples, method names, property names, or code snippets where relevant
- Gotchas must describe the actual failure mode and the correct approach
- Cross-references must name a real existing skill and explain the relationship

---

## Domain Naming Conventions

| Convention | Details |
|------------|---------|
| Format | Lowercase, hyphen-separated |
| Granularity | Domain/framework/layer level — not per-fact |
| Scope prefix | For project skills: prefix with project name if ambiguity possible, e.g. `projectname-architecture` |

**Global examples**: `maui-navigation`, `signalr-hubs`, `dotnet-patterns`, `maui-controls`, `csharp-async`

**Project examples**: `projectname-architecture`, `projectname-data-models`, `projectname-game-flow`, `projectname-server-hubs`

**Anti-patterns**: `reconnection-fix`, `my-notes`, `misc`, `todo` — too narrow or non-descriptive

---

## Memory System Boundary

| System | What Goes Here | Lifetime |
|--------|---------------|----------|
| `/memories/session/` | In-progress task notes, current plan, temporary context | Current session only |
| `/memories/repo/` | Quick project facts, build commands, local conventions not worth a full skill | Persistent but informal |
| `/memories/` (user) | Cross-workspace preferences and patterns | Persistent |
| **Learn skill outputs** | Curated, structured knowledge for future agent discovery | Permanent, indexed by skill description |

If something is genuinely reusable by a future agent starting cold — it belongs in a knowledge skill, not just memory.

---

## Approval Is Always Required

There are no exceptions to the approval requirement. This applies to:
- Creating a new skill folder and SKILL.md
- Adding content to an existing skill's SKILL.md
- Creating a new reference file within an existing skill
- Reorganizing, renaming, or deleting any skill file

Always use `vscode_askQuestions` and wait for an explicit approval response before writing.
