---
name: "Continue Build"
description: "Resume development with full project context. Use when starting a new session, picking up where you left off, or deciding what to work on next."
---

# Continue Build

## What This Skill Does

Gathers full project context and helps you pick up exactly where the last session ended.

## Procedure

### 1. Gather Context (do all in parallel)

- Read `CLAUDE.md` for project overview, conventions, and design philosophy
- Read `.taskmaster/queue.json` for pending tasks
- Run `git log --oneline -15` for recent commits
- Run `git diff --stat` for uncommitted changes
- Run `gh pr list --state open --limit 5` for open PRs
- Check Claude memory for session notes

### 2. Present Status

Show a concise status report:

```
Last session:
  [Most recent commits — what was accomplished]

Uncommitted changes:
  [Files modified but not committed, if any]

Open PRs:
  [List or "None"]

Pending tasks (next up):
  [Top 3-5 tasks from queue.json, grouped by phase]

Suggested next step:
  [The highest-priority pending task with rationale]
```

### 3. Execute

Ask the user which task to tackle (or confirm the suggestion). Then:

1. Execute the chosen task
2. Run tests after code changes
3. Update `.taskmaster/queue.json`:
   - Mark completed task as `"done"`
   - Add any newly discovered tasks as `"pending"`
4. Summarize what was done and what's next
