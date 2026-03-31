---
name: init-project
description: Bootstrap any new project with self-learning agent structure — CLAUDE.md, task queue, skills, and session continuity
user_invocable: true
---

You are setting up a **self-learning project structure** so that every Claude session picks up where the last one left off and keeps improving the app.

## Step 1 — Understand the project

Ask the user:
1. **Project name** and one-line description
2. **Tech stack** (framework, DB, styling, APIs)
3. **Design philosophy** (e.g., "Liquid Glass / Apple-inspired", "Material Design", "Brutalist", etc.)
4. **Target audience / language** (e.g., Spanish for Mexico, English for US)
5. **Phases** — what are the 3-5 major milestones to launch?

## Step 2 — Create CLAUDE.md

Read the project's actual files (package.json, config files, existing code) and generate a `CLAUDE.md` at the project root with:
- Quick start commands
- Tech stack with exact versions
- Design tokens / brand palette (from tailwind.config, theme files, or user input)
- File structure map
- Database schema (if applicable)
- API patterns and key routes
- Auth flow
- Coding conventions and "Do NOT" rules
- Design philosophy section

This file is the **single source of truth** that Claude reads every session.

## Step 3 — Create .taskmaster/

Create the task queue and config:

**`.taskmaster/config.json`**:
```json
{
  "models": {
    "main": {
      "provider": "anthropic",
      "modelId": "claude-sonnet-4-6",
      "maxTokens": 128000,
      "temperature": 0.2
    },
    "architect": {
      "provider": "anthropic",
      "modelId": "claude-opus-4-6",
      "maxTokens": 128000,
      "temperature": 0.3
    },
    "fallback": {
      "provider": "anthropic",
      "modelId": "claude-haiku-4-5-20251001",
      "maxTokens": 64000,
      "temperature": 0.1
    }
  },
  "global": {
    "logLevel": "info",
    "debug": false,
    "projectName": "PROJECT_NAME",
    "responseLanguage": "LANGUAGE",
    "enableCodebaseAnalysis": true,
    "defaultTag": "master"
  }
}
```

**`.taskmaster/queue.json`**: Convert the user's phases into structured tasks:
```json
[
  {
    "id": "P1-001",
    "description": "Task description",
    "status": "pending",
    "phase": 1
  }
]
```
Break each phase into 3-6 concrete tasks. Mark nothing as completed unless it already exists in the codebase.

## Step 4 — Create project-level skills

Create `.claude/skills/` with these three skills:

### `.claude/skills/continue-build.md`
A skill that:
1. Reads `CLAUDE.md` for project context
2. Reads `.taskmaster/queue.json` for pending tasks
3. Checks `git log --oneline -15` for recent work
4. Checks Claude memory for session notes
5. Checks open PRs with `gh pr list --state open`
6. Presents: last completed work + pending tasks + suggested next steps
7. Asks which task to tackle, then executes it
8. After completing, updates queue.json (mark done, add discovered tasks)

### `.claude/skills/audit-ui.md`
A skill that:
1. Reads the target file the user specifies
2. Evaluates against the project's design philosophy from CLAUDE.md
3. Scores /10 with issues (critical/warning/suggestion)
4. Offers concrete code fixes
5. Asks if user wants auto-apply

### `.claude/skills/new-page.md`
A skill that scaffolds a new page/route following the project's exact conventions:
- Framework patterns (Next.js pages, React Router, etc.)
- Design system components
- Auth integration if needed
- Correct language for UI text

## Step 5 — Update .gitignore

Ensure `.taskmaster/` is in `.gitignore` (it's local state, not code).

## Step 6 — Confirm

Show the user a summary:
- Files created (CLAUDE.md, queue.json, config.json, 3 skills)
- Total tasks queued by phase
- How to use: `/continue-build` to resume, `/audit-ui` to review, `/new-page` to scaffold
- Remind them: every new session, Claude reads CLAUDE.md automatically. Use `/continue-build` to get the full context of where things stand.
