---
name: "Init Project"
description: "Bootstrap a new project with self-learning agent structure: CLAUDE.md, task queue, project skills, and session continuity. Use when starting a new project, initializing a codebase for Claude, setting up task tracking, or onboarding an existing repo for AI-assisted development."
---

# Init Project

## What This Skill Does

Sets up a complete self-learning project structure so every Claude session picks up where the last left off. Creates:
1. `CLAUDE.md` — single source of truth for project context
2. `.taskmaster/` — task queue with phased milestones
3. `.claude/skills/` — project-level skills for continuity

## Prerequisites

Before running, ensure you have:
- `git` initialized in the project directory
- `gh` CLI installed (for PR-aware skills)
- Your framework/tooling already scaffolded (Next.js, Remix, Shopify, etc.)

---

## Step 1 — Detect Existing Codebase

Before asking the user anything, scan the project:

1. Read `package.json` (or equivalent) for name, dependencies, scripts
2. Read framework config files: `next.config.*`, `remix.config.*`, `vite.config.*`, `shopify.theme.toml`, `tailwind.config.*`, `tsconfig.json`
3. Read existing `CLAUDE.md` if present (avoid overwriting)
4. Read `.env.example` or `.env.local.example` for env var names (never read `.env` itself)
5. Check `git log --oneline -5` for recent activity
6. Run `ls -la` on root to map file structure

Use what you find to pre-fill answers. Only ask the user what you can't detect.

## Step 2 — Ask the User (Only Unknowns)

Present what you detected, then ask only what's missing:

1. **Project name** — pre-fill from `package.json` name if found
2. **One-line description** — what does this project do?
3. **Tech stack** — pre-fill from dependencies, confirm with user
4. **Design philosophy** — e.g., "Apple Liquid Glass", "Brutalist", "Material Design" (check for Tailwind theme, CSS variables, or design tokens first)
5. **Target audience / language** — e.g., "Spanish for Mexico", "English for US"
6. **Phases** — what are the 3-5 major milestones to launch?

Format the question as a concise checklist showing detected values with `[detected]` tags so the user can confirm or correct.

## Step 3 — Create CLAUDE.md

Generate `CLAUDE.md` at the project root. Pull real data from the codebase — do not use placeholders.

Structure:

```markdown
# Project Name

> One-line description

## Quick Start

\`\`\`bash
# Install
[detected install command]

# Dev
[detected dev command]

# Build
[detected build command]

# Test
[detected test command]
\`\`\`

## Tech Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | [detected] | [detected] |
| Styling | [detected] | [detected] |
| Database | [detected or user input] | [detected] |
| Auth | [detected or user input] | — |
| Deployment | [detected or user input] | — |

## Design Philosophy

[User's stated design philosophy]

### Brand Tokens
[Extract from tailwind.config, CSS variables, or theme files. If none exist, note "No design tokens detected — define in tailwind.config or CSS variables."]

## File Structure

\`\`\`
[Actual tree output, max 3 levels deep, excluding node_modules/.git]
\`\`\`

## Database Schema

[If detected from prisma.schema, drizzle config, SQL files, or migrations. Otherwise: "No schema detected."]

## API Patterns

[If detected from routes/, api/, or server files. Otherwise: "No API routes detected."]

## Auth Flow

[If detected. Otherwise: "No auth detected."]

## Conventions

- [Derive from existing code patterns: naming, file organization, import style]
- [Note any .eslintrc, .prettierrc, or biome.json rules]

## Do NOT

- Do not modify [critical files detected from codebase]
- Do not hardcode secrets — use environment variables
- Do not skip tests before committing

## Phases

| Phase | Milestone | Status |
|-------|-----------|--------|
| 1 | [from user] | pending |
| 2 | [from user] | pending |
| ... | ... | ... |
```

## Step 4 — Create .taskmaster/

Create two files:

### `.taskmaster/config.json`

```json
{
  "models": {
    "main": {
      "provider": "anthropic",
      "modelId": "claude-sonnet-4-6"
    },
    "architect": {
      "provider": "anthropic",
      "modelId": "claude-opus-4-6"
    },
    "fast": {
      "provider": "anthropic",
      "modelId": "claude-haiku-4-5-20251001"
    }
  },
  "global": {
    "projectName": "PROJECT_NAME",
    "responseLanguage": "LANGUAGE"
  }
}
```

Replace `PROJECT_NAME` and `LANGUAGE` with actual values from Step 2.

### `.taskmaster/queue.json`

Convert the user's phases into structured tasks. Break each phase into 3-6 concrete, actionable tasks:

```json
[
  {
    "id": "P1-001",
    "description": "Clear, actionable task description",
    "status": "pending",
    "phase": 1,
    "priority": "high"
  }
]
```

Rules:
- Only mark tasks `"done"` if they verifiably exist in the codebase right now
- Use IDs like `P1-001`, `P2-003` (phase-sequence)
- Set priority: `"high"` for blockers, `"medium"` for features, `"low"` for polish

## Step 5 — Create Project-Level Skills

Create `.claude/skills/` at the project root with three skills. Use the templates in `resources/templates/` as the exact content for each file:

| Skill | File | Purpose |
|-------|------|---------|
| Continue Build | `.claude/skills/continue-build/SKILL.md` | Resume work with full context |
| Audit UI | `.claude/skills/audit-ui/SKILL.md` | Score UI against design philosophy |
| New Page | `.claude/skills/new-page/SKILL.md` | Scaffold pages following conventions |

See [Continue Build Template](resources/templates/continue-build.md), [Audit UI Template](resources/templates/audit-ui.md), and [New Page Template](resources/templates/new-page.md) for the exact content to write.

## Step 6 — Update .gitignore

Append to `.gitignore` if not already present:

```
# Task queue (local state)
.taskmaster/
```

Do NOT gitignore `.claude/skills/` — those should be committed so the team shares them.

## Step 7 — Confirm

Show the user a summary table:

```
Files created:
  CLAUDE.md              — project context (single source of truth)
  .taskmaster/config.json — model config
  .taskmaster/queue.json  — [N] tasks across [M] phases
  .claude/skills/continue-build/SKILL.md
  .claude/skills/audit-ui/SKILL.md
  .claude/skills/new-page/SKILL.md
  .gitignore             — updated

How to use:
  /continue-build  — resume where you left off
  /audit-ui        — review any file against your design system
  /new-page        — scaffold a new page/route

Every new Claude session automatically reads CLAUDE.md.
Use /continue-build to get full context of pending work.
```

---

## Troubleshooting

### CLAUDE.md already exists
Do NOT overwrite. Read it, show the user what's there, and ask if they want to merge or replace.

### No package.json found
Ask the user for stack details manually. This skill works for any language/framework — adapt the config detection accordingly.

### Skills don't appear in Claude
Verify the directory structure: `.claude/skills/[skill-name]/SKILL.md`. Flat files like `.claude/skills/foo.md` are not detected. Restart Claude Code after creating skills.
