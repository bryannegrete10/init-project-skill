---
name: "Audit UI"
description: "Score a UI file against the project's design philosophy and conventions. Use when reviewing components, pages, or templates for design consistency."
---

# Audit UI

## What This Skill Does

Evaluates a file against the project's design system and scores it with actionable fixes.

## Procedure

### 1. Read Context

- Read `CLAUDE.md` — focus on Design Philosophy, Brand Tokens, and Conventions sections
- Read the target file the user specifies

### 2. Evaluate

Score the file out of 10 across these dimensions:

| Dimension | What to Check |
|-----------|--------------|
| Design consistency | Does it follow the stated design philosophy? |
| Spacing & layout | Consistent use of spacing scale, grid, alignment |
| Typography | Correct font families, sizes, weights from tokens |
| Color usage | Uses brand palette, proper contrast ratios |
| Responsiveness | Mobile-first, breakpoints match design system |
| Accessibility | Alt text, ARIA labels, keyboard navigation, contrast |

### 3. Report

Present findings grouped by severity:

- **Critical** — Breaks design system or accessibility (must fix)
- **Warning** — Inconsistent with conventions (should fix)
- **Suggestion** — Could improve but acceptable as-is

For each issue, show:
- Line number and current code
- What's wrong
- Concrete fix (exact code replacement)

### 4. Auto-Apply

Ask: "Want me to apply these fixes?" If yes, apply all critical and warning fixes. Leave suggestions for user decision.
