# /init-project — Self-Learning Claude Code Skill

A global Claude Code skill that bootstraps **any project** with a self-learning agent structure. Every session picks up where the last one left off.

## What it does

When you run `/init-project` in any directory, Claude will:

1. **Detect your codebase** — reads `package.json`, framework configs, Tailwind theme, git history
2. **Ask only what's missing** — confirms detected stack, asks for unknowns
3. Generate **`CLAUDE.md`** — single source of truth for architecture, conventions, and design tokens
4. Create **`.taskmaster/queue.json`** — phased roadmap with trackable tasks
5. Create **3 project-level skills**:
   - `/continue-build` — resume from where the last session ended
   - `/audit-ui` — design audit scored /10 with auto-fix
   - `/new-page` — scaffold pages following your exact conventions

## Install

### Option A — Clone and copy

```bash
git clone https://github.com/bryannegrete10/init-project-skill.git
cp -r init-project-skill ~/.claude/skills/init-project
```

### Option B — Clone and symlink

```bash
git clone https://github.com/bryannegrete10/init-project-skill.git
ln -s "$(pwd)/init-project-skill" ~/.claude/skills/init-project
```

## File Structure

```
init-project/
├── SKILL.md                          # Main skill (Claude reads this)
├── README.md                         # This file
├── LICENSE
└── resources/
    └── templates/
        ├── continue-build.md         # Template for /continue-build skill
        ├── audit-ui.md               # Template for /audit-ui skill
        └── new-page.md               # Template for /new-page skill
```

## Usage

```
cd ~/my-new-app
claude
> /init-project
```

Then every future session:

```
> /continue-build
```

Claude reads CLAUDE.md + queue.json + git log → knows exactly where things stand → builds the next task → updates the queue → next session picks up seamlessly.

## How it works

```
┌─────────────────────────────────────────────────┐
│              /init-project                       │
│  1. Scans codebase (package.json, configs, git)  │
│  2. Asks only what it can't detect               │
│  3. Creates the self-learning structure:         │
│                                                  │
│  CLAUDE.md ─────── Architecture & conventions    │
│  .taskmaster/ ──── Task queue & agent config     │
│  .claude/skills/ ─ Project-specific skills       │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│            /continue-build                       │
│  Every new session:                              │
│                                                  │
│  1. Read CLAUDE.md (context)                     │
│  2. Read queue.json (pending tasks)              │
│  3. Read git log (recent work)                   │
│  4. Present status + suggest next task           │
│  5. Build it                                     │
│  6. Update queue.json                            │
│  7. Next session picks up here ↩                 │
└─────────────────────────────────────────────────┘
```

## Requirements

- [Claude Code](https://claude.ai/code) CLI or Desktop App
- Git (for tracking progress across sessions)
- `gh` CLI (optional, for PR-aware features)

## License

MIT
