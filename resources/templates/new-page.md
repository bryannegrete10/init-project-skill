---
name: "New Page"
description: "Scaffold a new page or route following the project's exact conventions. Use when creating new pages, adding routes, or scaffolding views."
---

# New Page

## What This Skill Does

Creates a new page/route that matches the project's framework patterns, design system, and conventions exactly.

## Procedure

### 1. Read Context

- Read `CLAUDE.md` — Tech Stack, File Structure, Conventions, Design Philosophy
- Read an existing page/route as a reference pattern (pick the most recent or most complete one)

### 2. Ask the User

1. **Page name / route** — e.g., "About", "/products/:id"
2. **Purpose** — one-line description of what this page does
3. **Data needs** — does it fetch data? From where? (API, DB, CMS)
4. **Auth required?** — yes/no

### 3. Scaffold

Create all necessary files following the detected framework pattern:

**Next.js App Router**: `app/[route]/page.tsx` + `layout.tsx` if needed
**Next.js Pages**: `pages/[route].tsx`
**Remix**: `app/routes/[route].tsx` with loader/action
**React Router**: Component + route config entry
**Shopify Liquid**: `templates/page.[name].liquid` or `sections/`
**Other**: Match whatever pattern exists in the codebase

Include:
- Correct imports matching project conventions
- Design system components (not raw HTML)
- Proper TypeScript types if the project uses TS
- UI text in the project's target language
- Loading/error states if data fetching
- Auth guard if required
- SEO meta tags if the framework supports them

### 4. Verify

- Run the dev server or build to confirm no errors
- Show the user what was created and where
