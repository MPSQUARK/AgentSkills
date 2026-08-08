# AgentSkills

Repository of AI agent skills for structured discovery, alignment, and implementation.

## Install

**Cursor** (global):

- `~/.cursor/skills/`
- `~/.agents/skills/`
- Windows: `C:\Users\<user>\.cursor\skills` or `C:\Users\<user>\.agents\skills`

**GitHub Copilot** (global): `~/.copilot/skills/`

Copy or symlink skill folders into the appropriate directory for your agent.

## Skill workflow

- **`design-document-discovery`** — domain/system specs via repo-wide discovery and vision-vs-code alignment
- **`clarify-requirements`** — feature-scoped plans before implementation (`/memories/session/plan.md` in Cursor)
- **`gauntlet-loop`** — ambitious artifacts via builder + critic loops (`/gauntlet-loop`; spec at `/memories/session/gauntlet-loop.md`)
- **`code`**, **`frontend-design`**, **`learn`** — implementation quality, UI/UX, and durable knowledge capture

Run domain discovery first for a new area; use clarify-requirements for each feature against that spec. Use gauntlet-loop when the goal is ambitious, has an inspectable quality bar, and benefits from sustained build–criticize–improve iteration.
