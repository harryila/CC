# Claude Code — Complete Guide for Harry

> Everything about capabilities, installed tools, commands, skills, and how to use them effectively.
> Created: February 2026

---

## What's Currently Installed

### Plugins (2)

| Plugin | Version | What it does |
|--------|---------|-------------|
| **superpowers** | 4.2.0 | Enforces plan→implement→test→review workflows, TDD, systematic debugging, code review |
| **planning-with-files** | 2.11.0 | Creates persistent `task_plan.md`, `findings.md`, `progress.md`; auto-re-reads plan before every action |

### MCP Servers (3)

| Server | Scope | What it does |
|--------|-------|-------------|
| **chrome-devtools** | Global (all projects) | Browser automation, debugging, screenshots, performance profiling |
| **context7** | Per-project (`~/`) | Live library documentation lookup |
| **repomix** | Per-project (`~/`) | Pack codebases into AI-friendly single files |

> **Note:** context7 and repomix are only configured under `~/` — they won't be available in projects outside your home directory unless you add them globally. chrome-devtools is global.

---

## All Built-in Slash Commands

These ship with Claude Code — you can't change them:

| Command | What it does |
|---------|-------------|
| `/clear` | Wipe conversation history (fresh start) |
| `/compact [focus]` | Compress conversation to save context window, optionally focusing on a topic |
| `/config` | Open settings UI |
| `/context` | Visual grid showing how much context window is used |
| `/cost` | Token usage and cost breakdown |
| `/debug` | Read session debug log for troubleshooting |
| `/doctor` | Health check your installation |
| `/exit` | Quit |
| `/export [file]` | Export conversation to file or clipboard |
| `/help` | Usage help |
| `/init` | Generate a CLAUDE.md for the current project |
| `/mcp` | Manage MCP server connections |
| `/memory` | Edit your CLAUDE.md memory files |
| `/model` | Switch models (Opus/Sonnet/Haiku) or adjust effort |
| `/permissions` | View/edit what tools Claude can auto-approve |
| `/plan` | Enter plan mode |
| `/rename` | Rename current session |
| `/resume` | Resume a previous session |
| `/rewind` | Roll back conversation or code changes |
| `/stats` | Usage visualizations, streaks |
| `/status` | Version, model, account info |
| `/statusline` | Configure the status bar |
| `/copy` | Copy last response to clipboard |
| `/tasks` | Manage background tasks |
| `/teleport` | Resume a remote session from claude.ai |
| `/theme` | Change colors |
| `/todos` | View TODO list |
| `/usage` | Subscription rate limits |
| `/vim` | Enable vim keybindings |

---

## Skills From Installed Plugins

These activate automatically when relevant, or can be invoked manually via `/skill-name`.

### From superpowers

| Skill | What it does |
|-------|-------------|
| `/superpowers:brainstorming` | Design exploration before building |
| `/superpowers:writing-plans` | Structured implementation plans |
| `/superpowers:executing-plans` | Execute plans with review checkpoints |
| `/superpowers:test-driven-development` | RED-GREEN-REFACTOR enforcement |
| `/superpowers:systematic-debugging` | 4-phase root cause analysis |
| `/superpowers:dispatching-parallel-agents` | Run multiple subagents simultaneously |
| `/superpowers:subagent-driven-development` | Autonomous execution with review |
| `/superpowers:verification-before-completion` | Gate: prove it works before claiming done |
| `/superpowers:requesting-code-review` | Get code reviewed |
| `/superpowers:receiving-code-review` | Respond to review feedback rigorously |
| `/superpowers:using-git-worktrees` | Isolated feature branches |
| `/superpowers:finishing-a-development-branch` | Merge/PR/cleanup guidance |
| `/superpowers:writing-skills` | Create new skills |

### From planning-with-files

| Skill | What it does |
|-------|-------------|
| `/planning-with-files:plan` | Start file-based planning (creates task_plan.md, findings.md, progress.md) |
| `/planning-with-files:status` | Check planning progress at a glance |

---

## Slash Commands vs Skills vs Plugins

Three different layers of extensibility:

### Built-in Slash Commands

- **What**: Pre-built commands shipped with Claude Code (`/clear`, `/cost`, `/model`, etc.)
- **How**: Type `/` and select from the list
- **Who controls**: Anthropic
- **Customizable**: No — hardcoded features

### Skills

- **What**: Custom extensions that teach Claude how to do things
- **How**: Invoke with `/skill-name` or Claude invokes them automatically when relevant
- **Who controls**: You create and manage them
- **Where they live**:
  - Personal: `~/.claude/skills/<skill-name>/SKILL.md` (all your projects)
  - Project: `.claude/skills/<skill-name>/SKILL.md` (current project only)
  - Plugin: Inside a plugin's `skills/` directory
- **Key point**: Skills are extensible templates for Claude's behavior

### Plugins

- **What**: Distributable packages bundling skills, agents, hooks, MCP servers, and LSP servers
- **How**: Install from marketplaces with `/plugin install`, or load locally with `--plugin-dir`
- **Who controls**: Developers create, users install
- **Contains**: Skills, agents, hooks, MCP servers, LSP servers
- **Key point**: Plugins are namespaced (`/plugin-name:skill-name`) and are the distribution mechanism

### Comparison Table

| Aspect | Built-in Commands | Skills | Plugins |
|--------|------------------|--------|---------|
| **Created by** | Anthropic | You | You or others |
| **Invocation** | `/command` | `/skill-name` or automatic | `/plugin:skill` or auto |
| **Location** | Built-in | `.claude/skills/` | Shared directory + marketplace |
| **Shareable** | N/A | Manually copy or commit | Through marketplaces |
| **Scope** | All sessions | Personal/project/plugin | Across projects & teams |

---

## Keyboard Shortcuts

### General Controls

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input or generation |
| `Ctrl+D` | Exit Claude Code session |
| `Ctrl+G` | Open prompt in default text editor |
| `Ctrl+L` | Clear terminal screen (keeps history) |
| `Ctrl+O` | Toggle verbose output (shows detailed tool usage) |
| `Ctrl+R` | Reverse search command history |
| `Ctrl+V` / `Cmd+V` | Paste image from clipboard |
| `Ctrl+T` | Toggle task list |
| `Esc` + `Esc` | Rewind or summarize conversation |
| `Shift+Tab` / `Alt+M` | Toggle permission modes (Auto-Accept / Plan / Normal) |
| `Option+P` (Mac) / `Alt+P` | Switch AI model without clearing prompt |
| `Option+T` (Mac) / `Alt+T` | Toggle extended thinking mode |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+K` | Delete to end of line |
| `Ctrl+U` | Delete entire line |
| `Ctrl+Y` | Paste deleted text |
| `Alt+B` | Move cursor back one word |
| `Alt+F` | Move cursor forward one word |

### Multiline Input

| Method | How |
|--------|-----|
| Universal | `\ + Enter` (works in all terminals) |
| macOS default | `Option+Enter` |
| iTerm2/WezTerm/Ghostty/Kitty | `Shift+Enter` |
| Line feed | `Ctrl+J` |

### Quick Prefixes

| Prefix | What it does |
|--------|-------------|
| `/` at start | Open command/skill menu |
| `!` at start | Bash mode — run shell commands directly without Claude interpreting |
| `@` | File path mention/autocomplete |

### macOS Terminal Setup

For `Alt` shortcuts to work on macOS, configure Option as Meta:
- **iTerm2**: Settings → Profiles → Keys → Set Left/Right Option key to "Esc+"
- **Terminal.app**: Settings → Profiles → Keyboard → Check "Use Option as Meta Key"

---

## What to Master for the Jarvis Vision

Ranked by daily importance given the autonomous AI dev system plan.

### Tier 1 — Use Every Session

| Command/Skill | Why |
|---------------|-----|
| `/plan` or `/superpowers:writing-plans` | Plan before coding, always |
| `/compact` | When context gets long, compress it (you'll hit limits on big tasks) |
| `/clear` | Fresh start when switching tasks |
| `/resume` | Pick up where you left off |
| `@filename` | Reference files inline (faster than describing them) |

### Tier 2 — Use for Serious Work

| Command/Skill | Why |
|---------------|-----|
| `/superpowers:test-driven-development` | Write tests first, then implement |
| `/superpowers:systematic-debugging` | Don't guess at bugs — systematic root cause analysis |
| `/superpowers:dispatching-parallel-agents` | Run multiple agents on independent subtasks |
| `/superpowers:verification-before-completion` | Critical: forces proof that things work |
| `/permissions` | Set up auto-approve for tools you trust (saves confirmation clicks) |

### Tier 3 — Know They Exist

| Command/Skill | Why |
|---------------|-----|
| `/model` | Switch to Haiku for cheap exploration, Opus for hard problems |
| `/export` | Save important conversations |
| `/rewind` | Undo when Claude goes down a wrong path |
| `Shift+Tab` | Toggle between normal/auto-accept/plan mode (very useful) |
| `!command` | Prefix with `!` to run bash directly |

---

## The "Soul" CLAUDE.md

CLAUDE.md files cascade — multiple levels are loaded together:

```
~/.claude/CLAUDE.md                        ← "Soul" — applies to ALL projects
  └── ~/projects/app-a/CLAUDE.md           ← Project-specific overrides
  └── ~/projects/app-b/CLAUDE.md           ← Project-specific overrides
      └── ~/projects/app-b/src/CLAUDE.md   ← Subdirectory-specific (optional)
```

### What the Soul Should Contain

The root `~/.claude/CLAUDE.md` ("soul") applies to every project and should contain:

- Your identity/preferences as a developer
- Workflow rules (always plan before coding, always TDD, etc.)
- How to create CLAUDE.md files for new projects
- Which tools/patterns to prefer
- Communication style preferences
- Review/checkpoint requirements

### What Project CLAUDE.md Files Should Contain

Each project-level CLAUDE.md should contain project-specific things:

- Tech stack and framework versions
- Build, test, and lint commands
- Architecture overview
- Code conventions and patterns
- Important file paths

### How They Work Together

Both files are loaded simultaneously. The soul provides the operating principles, the project file provides the specifics. Claude sees both and follows both.

---

## Running Multiple Instances in Parallel

**Yes — this is a core use case.** Each terminal runs an independent Claude Code instance.

```
Terminal 1:  cd ~/projects/app-a && claude    ← working on frontend
Terminal 2:  cd ~/projects/app-b && claude    ← working on API
Terminal 3:  cd ~/projects/app-c && claude    ← running tests/debugging
```

### What Each Instance Gets

| Resource | Shared or Independent? |
|----------|----------------------|
| Conversation history | Independent — each has its own |
| Context window | Independent — each has its own full window |
| Working directory | Independent — each uses its own `cd` path |
| Model selection | Independent — each can use different models |
| `~/.claude/CLAUDE.md` (soul) | Shared — all instances load it |
| Project CLAUDE.md | Independent — each loads the one in its working directory |
| MCP servers | Shared — chrome-devtools, context7, repomix available to all |
| Plugins | Shared — superpowers, planning-with-files active in all |
| Subscription rate limit | **Shared** — 3 heavy sessions hit limits faster |
| Git operations | **Can conflict** — use worktrees to isolate |

### Avoiding Git Conflicts

If two instances try to commit or branch in the same repo, they'll conflict. Solutions:

1. **Git worktrees** (`/superpowers:using-git-worktrees`) — each instance works in an isolated worktree of the same repo
2. **Different repos** — each instance works in a completely different project
3. **Different branches** — coordinate so instances don't touch the same branch

### Rate Limits

Your subscription rate limit is shared across all running instances. Running 3 heavy sessions simultaneously will consume your quota ~3x faster. Strategies:

- Use Haiku for exploration/simple tasks (costs much less)
- Use Opus only for complex reasoning tasks
- `/usage` to check remaining quota
- `/model` to switch models per-instance based on task complexity

### This Is Phase 3 of Your Plan

You don't need oh-my-claudecode to do basic parallel work — just open multiple terminals. Oh-my-claudecode adds *coordination between them* (shared context, smart routing, magic keywords like "autopilot" and "ultrawork"). That's the Phase 3 upgrade.
