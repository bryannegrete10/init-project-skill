# /init-project — Self-Learning Claude Code Skill

A global Claude Code skill that bootstraps **any project** with a self-learning agent structure. Every session picks up where the last one left off.

## What it does

When you run `/init-project` in any directory, Claude will:

1. Ask about your project (name, stack, design philosophy, phases)
2. Read your actual codebase
3. Generate **`CLAUDE.md`** — single source of truth for architecture, conventions, and design tokens
4. Create **`.taskmaster/queue.json`** — phased roadmap with trackable tasks
5. Create **3 project-level skills**:
   - `/continue-build` — resume from where the last session ended
   - `/audit-ui` — design audit scored /10 with auto-fix
   - `/new-page` — scaffold pages following your exact conventions

## Install

### Option A — Copy the file

```bash
mkdir -p ~/.claude/skills
cp init-project.md ~/.claude/skills/
```

### Option B — Clone and symlink

```bash
git clone https://github.com/bryannegrete10/init-project-skill.git
ln -s "$(pwd)/init-project-skill/init-project.md" ~/.claude/skills/init-project.md
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
│  Creates the self-learning structure:            │
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

## License

MIT
