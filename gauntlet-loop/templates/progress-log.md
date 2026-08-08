# Gauntlet Progress Log

**Run:** [Gauntlet Loop name]
**Started:** [ISO timestamp]
**Spec:** `/memories/session/gauntlet-loop.md`

## Budget Tracker

| Resource | Limit | Used | Remaining |
|----------|-------|------|-----------|
| Time | | | |
| Rounds (total) | | | |
| Rounds per piece max | | | |

## Piece Status Snapshot

| ID | Piece | Status | Rounds | Last verdict |
|----|-------|--------|--------|--------------|
| P1 | | pending | 0 | — |

---

## Round Log

Append one entry per builder/critic round. Newest at top.

### [ISO timestamp] — Piece [ID] — Round [N]

**Actor:** builder | critic | integration-critic

**What changed:**
- [Concrete changes or inspection performed]

**Evidence:**
- [Test output path, screenshot path, command result, diff summary]

**Critic verdict:** PASS | FAIL | BLOCKED

**Largest gap (if FAIL):**
- [Single most meaningful gap with evidence]

**Fix target (if FAIL):**
- [Concrete correction for next builder round]

**Failed approaches (do not repeat):**
- [What was tried and why it did not work]

**Remaining budget:**
- Time: [remaining]
- Rounds this piece: [N / max]

---

### [ISO timestamp] — Piece [ID] — Round [N-1]

[Previous round entry...]

## Failed Approaches (global)

| Piece | Approach | Why it failed | Date |
|-------|----------|---------------|------|
| | | | |

## Blockers Requiring Human Judgment

| Piece | Blocker | Status |
|-------|---------|--------|
| | | open / resolved |

## Integration Pass

| Timestamp | Verdict | Gaps found | Action taken |
|-----------|---------|------------|--------------|
| | | | |

## Final Summary

[Written when run stops]

- **Outcome:** success | partial | stopped-by-boundary | blocked
- **Pieces passed:** [N / total]
- **Honest assessment vs quality bar:** [gaps remaining]
- **Evidence links:** [screenshots, test results, artifacts]
