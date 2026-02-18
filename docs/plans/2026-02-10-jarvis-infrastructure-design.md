# Jarvis Infrastructure Design

> Personal AI development system — from idea library to autonomous execution.
> Status: DESIGNING
> Started: February 10, 2026

### Design Review Log

| Date | Reviewer | Outcome |
|------|----------|---------|
| 2026-02-10 | Cursor (review 1) | Validated core design. Flagged: soul loads everywhere (acceptable risk), iMessage too early (moved to Phase 2), STATUS.md staleness (resolved with regen rules), shared/ concurrent writes (hardened with one-writer rule). Identified gaps: budget tracking (added), backups (added), project CLAUDE.md template priority (acknowledged). |
| 2026-02-10 | Cursor (review 2) | Identified design flaw: soul symlink loads jarvis-specific rules everywhere. Fix: split into `~/.claude/CLAUDE.md` (universal) + `~/jarvis/CLAUDE.md` (jarvis-specific). Also flagged: journal won't scale as flat files (split by domain), STATUS.md auto-regen too eager (make on-demand), shared/ paths couple graduated projects to jarvis (shared/ for workbench only), soul needs size budget (~100 lines), idea template needs progressive sections by status. |
| 2026-02-10 | ChatGPT + OpenClaw | 9 items reviewed. Added: Cerebro inspiration (soul as constitution, permission to disagree, anti-filler), three-library structure (ideas/journal/memory), daily memory notes with session init, explicit model routing table, journal grouping logic, heartbeat via local LLM for Phase 2, rate limit guardrails as TODO, reference files for soul overflow. Soul identity section needs Harry's personal values (TODO). |
| 2026-02-10 | Research round 2 | Evaluated: Agent Teams, auto-memory, beads, Obsidian, daily-patterns-pack, claude-agent-borg, Cerebro deep dive. Decisions: added GitHub prerequisites, added auto-memory explanation + periodic review, dropped beads (redundant), skipped Obsidian (compatible but not needed), added Cerebro medium-priority items (escape hatches, project personality, file boundaries), added 2-week automation review, moved claude-agent-borg to Phase 2. |
| 2026-02-10 | Research round 3 | Evaluated: claude-code-voice-skill, CodeMoot (multi-model), WebMCP (Chrome 145/146), RTK (token savings), Agent Teams (detailed), git diff token tip. Decisions: merged Phase 3 into Phase 2 (no Phase 3 anymore), added voice skill to Phase 2, skipped CodeMoot (brainstorming own multi-model approach), added WebMCP as Phase 2 watch item, added RTK to Phase 1 prerequisites, added token efficiency rules to jarvis soul, added Agent Teams to Phase 2 with detail. |
| 2026-02-16 | Self-review round 4 | Full plan critique (10 items). Accepted: session init token budget note, projects stay inside jarvis (~/jarvis/projects/), Phase 2 tiered into A/B/C, Agent Teams hedge added, System Health Checks section added. Declined: journal is premature (keep it), shared/ is over-designed (keep it), design doc splitting (keep it). New design decisions: project location changed, workbench vs projects distinction marked TODO, shared data scope expanded to all jarvis projects. |
| 2026-02-16 | Brainstorm round 5 | Deep dive into connections.md. Identified three types of cross-project intelligence: structural (registry.json), knowledge (journal/), creative (new). connections.md was overengineered — duplicated registry data flows and journal patterns, grew indefinitely, required pre-work ritual. Replaced with opportunities.md: a self-cleaning triage queue for creative observations only. Added autopilot boundary fence, session init tight rules, kill/archive cadence. |
| 2026-02-16 | Cursor review 5 | Critical design review (13 items). Accepted: model routing rewrite (sub-agent delegation, not session switching), registry.json moved to Phase 1 (STATUS.md becomes a view on registry with full-scan fallback), daily memory auto-write (no approval gate), cross-project intelligence rewritten as connections.md system (replaces hope-based "proactive connection-making" with structured intelligence layer drawing from registry + ideas + journal + shared + status). Declined: session init hook (CLAUDE.md instruction is sufficient), tags system for cross-project intelligence (too static — connections.md is a living document instead). |

---

## Design Decisions Log

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Project location | **All inside jarvis** — small experiments in `~/jarvis/workbench/`, serious projects in `~/jarvis/projects/`. Projects only leave jarvis on explicit request. | Keeps jarvis soul loaded for all projects (cross-project intelligence, memory, journal access). No context gap when Claude runs in a serious project. The workbench/ vs projects/ distinction is TBD (TODO). |
| Git strategy | **Jarvis root is a repo, workbenches are gitignored** — each workbench/project has its own git repo | Ideas/docs are versioned together; project code doesn't pollute jarvis history |
| Project linking | **Path in idea file** — the idea file's `workbench:` field stores the path to the project (usually `workbench/<name>` or `projects/<name>`, both inside jarvis) | Single source of truth, no symlinks to maintain |
| Soul scope | **Split** — `~/.claude/CLAUDE.md` has universal principles (style, TDD, plan-before-code). `~/jarvis/CLAUDE.md` has jarvis-specific workflows (ideas, STATUS, journal, shared). Claude Code natively loads both when in jarvis. | Jarvis rules don't bleed into non-jarvis sessions. Universal principles apply everywhere. |
| Non-software ideas | **Same structure, all markdown** — every active idea gets a workbench with CLAUDE.md, even if it's just docs. 99% will involve code anyway | Consistent pattern; don't overthink categories |
| Inter-project data | **Shared data layer** — projects can produce/consume data through a convention (`~/jarvis/shared/` or API patterns) | Projects aren't fully isolated; one project's output can feed another |
| Idea creation | **Conversational** — Harry talks, Claude creates the file. Two modes: quick dump (2-3 questions) or full brainstorm. Harry picks in the moment. | Never write markdown by hand; the whole point of Claude is to do the structuring |
| Workflow codification | **Soul now, skills later** — soul describes behaviors in plain language. Extract into formal skills only when a pattern becomes repetitive enough to warrant it | No premature infrastructure; skills are overhead until proven necessary |
| Idea capture location | **Anywhere** — any Claude session can create idea files in `~/jarvis/ideas/` regardless of current directory. Plus iMessage/phone for mobile capture. | Zero friction; never lose an idea because you're in the wrong terminal or away from computer |
| Cross-project intelligence | **Active dot-connecting** — Claude doesn't just organize, it proactively sees connections between projects/ideas and proposes them. STATUS.md + group field + Claude initiative. | Claude should be a thinking partner, not just a filing system |
| Engineering memory | **Journal directory** — starts with `patterns.md` + `antipatterns.md`, splits by domain (e.g., `scraping.md`, `api-design.md`) when a file hits ~100 lines. Claude reads only relevant files. | Accumulate knowledge across all projects; avoid repeating mistakes; scales without becoming unreadable |
| Shared data scope | **Any jarvis project** — `shared/` is for communication between any projects inside `~/jarvis/` (both workbench/ and projects/). If a project ever leaves jarvis (rare, explicit), it must decouple from shared/ at that point. | Since projects stay inside jarvis by default, shared/ is available to all. Decoupling is only needed for the rare explicit extraction. |
| ***REVIEW*** Soul size budget ***REVIEW*** | **~100 lines max per file** — when `~/.claude/CLAUDE.md` or `~/jarvis/CLAUDE.md` grows past ~100 lines, extract detailed rules into reference files that Claude loads on-demand | Prevents soul from eating excessive context in every session |
| ***REVIEW*** STATUS.md regeneration ***REVIEW*** | **On-demand, not automatic** — Claude suggests regenerating during triage, or Harry asks. Auto-regen only when Claude is already modifying idea files (marginal cost). Not on every session start. | Avoids 15-30s latency penalty at session start as the system scales |
| Idea template growth | **Progressive sections** — seed template is minimal. As status progresses to exploring/ready, Claude adds architecture, tech stack, MVP scope sections through conversation. | Template matches idea maturity; don't front-load structure on half-formed thoughts |
| Context management | **Soul-driven** — soul tells Claude when/how to compress context, but ALWAYS ask before clearing | Optimize token usage without losing information Harry hasn't approved losing |
| Cerebro inspiration | **Cherry-pick ideas, not the system** — soul as constitution (values + personality), permission to disagree, anti-filler rules. Don't copy: Mac Mini infra, 47 agents, 8 MCP servers. Reference for Phase 2 only. | Cerebro validated the "soul = identity + principles" approach. Take the design philosophy, build our own implementation. |
| Soul identity structure | **Two halves for soul file** — identity (who Harry is, values, personality) + operating principles (how Claude should work). Cerebro's "four values" pattern is worth adopting. | Soul should feel like a person's constitution, not a system prompt. Identity half rarely changes; principles half evolves with usage. |
| Daily memory notes | **`memory/` directory, auto-written** — daily notes (`YYYY-MM-DD.md`) auto-written by Claude at session end (no approval gate), read at session start (today + yesterday). Lives alongside STATUS.md. Harry corrects next session if anything's wrong. | Approval gate created friction that led to skipping. An imperfect note that exists beats a perfect one that doesn't. Memory notes are ephemeral (1-2 day relevance) so the cost of a slightly wrong summary is near zero. |
| Three-library structure | **ideas/ + journal/ + memory/** — three distinct libraries with different purposes and lifecycles | Ideas = future (what to build). Journal = permanent (what we've learned). Memory = ephemeral (what's happening now, rolling window). |
| Model routing | **Sub-agent delegation for medium tasks, manual for session** — Main session model is a human choice at session start (default Sonnet, Opus when needed, switch via `/model`). For medium-weight delegated tasks (triage summaries, formatting, status compilation), spawn sub-agents on fast models via Task tool. Trivial tasks (quick git checks) stay on main session — sub-agent overhead isn't worth it for small things. Soul says: "For lightweight-to-medium tasks, delegate to a sub-agent on a fast model rather than doing it yourself." | Claude Code lets you specify model for sub-agents via Task tool but not switch the main session programmatically. Sub-agent spawning has startup overhead, so routing only saves cost/tokens for tasks with enough substance to offset that overhead. Session-level model choice remains a human decision. |
| Session initialization | **Read daily memory on jarvis session start** break line!!!! ***and soul and claude or is ther anything else to read?!!!***— on startup in jarvis, read today's + yesterday's memory notes to restore context | Cheap way to maintain continuity without loading full conversation history |
| Journal grouping logic | **Check-before-create + hierarchical splitting** — before creating a new domain file, check if an existing group covers it. Split groups further when they get long. | Prevents proliferation of tiny files. Groups stay coherent through explicit hierarchy. |
| GitHub prerequisites | **`gh` CLI + authentication** — prerequisite for workbench scaffolding (creating private repos). `gh auth login` must be done before jarvis can create remotes. | Plan assumed GitHub integration without specifying the tooling. Now explicit. |
| Auto-memory (built-in) | **Keep ON, complement our system** — Claude Code's auto-memory (`~/.claude/projects/.../memory/MEMORY.md`) is per-project AI-authored notes. Our `memory/` directory is cross-project human-curated context. Different scopes, no conflict. | Auto-memory handles project-level gotchas. Our memory/ handles system-level continuity. Periodically review what auto-memory saves. |
| Beads | **Dropped** — Claude Code's built-in TaskCreate/TaskUpdate + our idea files + STATUS.md already cover task tracking. Beads adds per-project issue boards that are redundant with this system. | Avoid installing tools that duplicate existing capabilities. |
| Anti-sycophancy | **Explicit duty in soul** — distinct from anti-filler. Claude must challenge bad ideas even when Harry is excited about them. "Don't let my enthusiasm override your judgment." | Permission to disagree is passive. Duty to challenge is active. Both needed. |
| Journal quality gate | **Textbook, not diary** — only add to journal/ when a lesson generalizes across future projects. Project-specific temporary details stay in daily memory notes. | Prevents journal pollution with transient project state. |
| Escape hatches | **Soul section for failure modes** — if going in circles, stop and say so. If context contradicts itself, flag it. If frustrated user, acknowledge it. | Prevents the common failure mode of Claude working on a wrong approach for too long. |
| Project personality | **"Stance" section in project CLAUDE.md** — each project gets a short personality: what to be cautious about, what to be loose about. One Claude adapts per project. | Cheaper than 47 named agents. Relevant to project CLAUDE.md template TODO. |
| File boundaries | **Explicit off-limits rules in soul** — never modify soul files without proposing changes, never delete idea files (archive instead), never write to shared/ folders you don't own. | Safety net against accidental destructive actions. |
| Automation review cycle | **Every 2 weeks** — review memory/ notes for recurring manual tasks that could be automated. Part of triage workflow. | Compounding effect: documentation becomes foundation for automation. Stolen from daily-patterns-pack concept. |
| Workbench vs projects distinction | **TODO** — both live inside `~/jarvis/`. The distinction between `workbench/` (experimental) and `projects/` (serious) needs to be figured out through real usage. What makes something "serious"? Quality gate? Scale? Complexity? TBD. | Don't over-specify this before using the system. Let real usage reveal the natural boundary. |
| RTK (token proxy) | **Phase 1 prerequisite** — Rust CLI proxy that intercepts Bash commands (git, ls, test runners, linters) and filters output before it reaches Claude's context. Saves 60-90% of tokens per session via smart filtering, grouping, truncation, deduplication. Installs as a PreToolUse hook. | Token efficiency is load-bearing for multi-terminal Jarvis. 70% savings per session = longer sessions, cheaper ops, more context window for actual work. 642 stars, 249 commits, actively maintained. |
| Token efficiency rules | **In jarvis soul** — explicit rules: no full `git diff` before commits (use `git status` + `git diff --stat`), selective file reads, no redundant reads. | A single `git diff` on 10-20 files wastes 10,000-50,000 tokens. `git diff --stat` achieves same awareness for ~100-500 tokens. 50-500x reduction on a frequent operation. |
| Voice skill | **Phase 2 bookmark** — claude-code-voice-skill enables phone conversations with Claude about your codebase via Vapi. Best for brainstorming while walking, quick status checks. | Cool factor is high. Not essential for workshop phase, but genuinely useful once 5+ workbenches are active. Low cost to add ($2/month Vapi). |
| CodeMoot | **Skipped (but brainstorming own approach)** — multi-model review tool bridging Claude + Codex. Adds complexity and second subscription. Will brainstorm whether a simpler DIY multi-model review is worth building. | 54 stars, 17 commits = very early. Extra subscription cost. Concept is interesting but unproven. |
| WebMCP | **Phase 2 watch** — W3C standard (Chrome 146 Canary) letting websites register structured tools for AI agents. Could eventually replace screenshot-based browser automation for certain workflows. | Still behind a flag, no website adoption yet. 6-12 months from usable. But validates browser-as-AI-tool-surface direction. |
| Agent Teams | **Phase 2 — enable flag now, build workflows later** — built-in Claude Code feature: shared task lists, direct messaging between agents, lifecycle control. Three layers stack: Agent Teams (engine) → oh-my-claudecode (wrapper) → wshobson/agents (templates). | The infrastructure that makes oh-my-claudecode and wshobson/agents more powerful. Enabling the flag is zero-risk. Building workflows is Phase 2. |
| Phase 3 elimination | **No Phase 3 — everything is Phase 1 or Phase 2** — merged all Phase 3 items (oh-my-claudecode, wshobson/agents) into Phase 2 | Simpler mental model. Two phases: build it (Phase 1), scale it (Phase 2). |
| Two-layer review system | **Layer 1: Claude reviews itself (internal). Layer 2: external model for genuinely different perspective (escalation).** Layer 1 uses superpowers code-review (Phase 1) + Agent Teams debate (Phase 2). Layer 2 uses a custom MCP server wrapping commercial APIs (Gemini, DeepSeek, OpenAI — start cheapest, design for many). Layer 2 only fires after Layer 1, and only for high-stakes or on explicit request. | Claude's own internal review catches 90% of issues. A different model's training biases and blind spots are genuinely different from Claude's — that's the value of Layer 2. But it should be an escalation, not the default. |
| Session initialization (tight) | **Tiered loading** — soul files load automatically. Memory: read today only (fall back to yesterday if today doesn't exist, never older unless asked). STATUS.md: only on explicit triage request, not by default. Model: cheap for triage/status/edits, strong for architecture/multi-file/debugging. | Prevents session start from becoming a token tax. Every load rule has a clear trigger — nothing is "just in case." |
| Kill / archive cadence | **Weekly forced decisions** — during triage, flag seed/exploring ideas untouched 30 days and active workbenches untouched 14 days. Each flagged item must be archived (default), paused (with review date), or promoted (with next action). Archiving marks status + reason, doesn't delete. | Without entropy pressure, the idea library becomes a graveyard. Forced decisions keep it lean and honest. |
| Session init token budget | **Cheap model for triage, Opus for deep work** — jarvis session start loads ~480 lines of context (two soul files + auto-memory + today/yesterday memory notes) before any work begins. Triage operations (check status, list active projects, quick idea dumps) should default to Haiku/Sonnet. Reserve Opus for sessions where you're doing actual deep work. | ~480 lines is ~2000 tokens of overhead. Acceptable for deep work sessions (small fraction of context). Wasteful if you're just asking "what's active?" Cheap model handles triage perfectly. |
| Registry (`registry.json`) | **Phase 1, single source of structured metadata** — machine-readable JSON index of all ideas/projects, updated incrementally when idea files change. STATUS.md is generated FROM registry (cheap), not from full idea file scans (expensive). Fallback: when explicitly asked or when registry seems stale, Claude does a full scan of idea files + git status to rebuild/verify the registry. | STATUS.md as primary data source requires O(n) full scans every time. registry.json is O(1) to query, O(1) to update one entry. STATUS.md becomes a rendered view of structured data, not the data itself. At 25 ideas + 10 projects, the difference between "read one JSON file" and "read 25 markdown files + check 10 git repos" is massive. |
| Cross-project intelligence | **Three-layer stack: registry.json (structural) + journal/ (knowledge) + opportunities.md (creative)** — structural connections (data flows, groups) live in registry.json. Knowledge connections (reusable patterns) live in journal/. Creative connections (opportunities, idea combinations) live in opportunities.md as a triage queue. Claude adds observations during work, Harry processes them during triage (act, dismiss, or defer). No pre-work ritual needed — opportunities surface during triage, not before every work session. | "Living intelligence document" (connections.md) was overengineered — it duplicated registry data flows and journal patterns, grew indefinitely, and required a pre-work ritual. The three-layer stack puts each type of cross-project intelligence in the system that already handles it, and adds opportunities.md only for the genuinely new thing: creative dot-connecting. Self-cleaning queue > permanent knowledge base for speculative insights. |
| Project CLAUDE.md template | **TODO** — draft exists but needs deeper brainstorming on what it should include and look like. Should include a "personality/stance" section (cautious about X, loose about Y). | Don't finalize yet; come back for dedicated design session |

---

## Overview

Build a personal "Jarvis" system around Claude Code that manages 10-25+ ideas across every category (software, research, experiments, content, business, automations, learning). The system must support three interaction modes:

1. **Autopilot** — Claude works autonomously, reports back with status. **Autopilot may execute bounded tasks; it may not change goals, roadmap, or product direction without approval.**
2. **Directed** — Harry gives direction, Claude executes, Harry reviews
3. **Hands-on** — Harry and Claude work together in real-time, with Cursor as the surgical cockpit for manual editing

### Approach

**Phase 1 (The Workshop):** Ideas library + isolated workbenches + soul CLAUDE.md + Cursor integration + shared data conventions + engineering journal. Get productive immediately.

**Phase 2 (The Studio):** Add orchestration, scheduling, background routines, multi-project coordination, iMessage integration, and advanced pipeline management on top of the Workshop foundation.

### Principles

- **Files you can read** — No databases, no hidden state. Everything is markdown, JSON, or code you can open and understand.
- **Claude builds it, Harry directs it** — Infrastructure is set up by Claude, maintained by Claude, but transparent enough that Harry can tweak anything.
- **Start sparse, grow dense** — An idea starts as a one-paragraph file. It only gets more structure when it needs it.
- **No premature infrastructure** — Don't build Phase 2 tooling until Phase 1 is full and creaking.
- **Cursor is the cockpit** — When Harry needs to go hands-on (edit files, review diffs, inspect visually), Cursor is the interface. Claude Code is the engine underneath.
- **Projects can talk** — Projects aren't fully siloed. When one project produces data another needs, there's a clean convention for sharing it.

### Cerebro Inspiration

The Cerebro project (Mac Mini running 47 specialized Claude agents with 8 MCP servers) validated several ideas worth stealing, and clarified what NOT to copy.

**Worth stealing:**
- **Soul as constitution, not system prompt** — The soul should read like a person's identity document: values, personality, principles. Not a list of instructions. Cerebro's "four values" pattern (curiosity, craftsmanship, honesty, pragmatism) gives the AI a character to inhabit, not just rules to follow.
- **Permission to disagree** — Claude should push back when it thinks Harry is making a mistake. The soul explicitly grants this. "If you think I'm wrong, say so."
- **Anti-filler rules** — Soul explicitly says: no fluff, no padding, no performative agreement. If something doesn't add value, don't say it.
- **Identity + operating principles** — Each soul file has two halves: identity (who Harry is, what he values — rarely changes) and operating principles (how Claude should work — evolves with usage). This mirrors Cerebro's split but without their infrastructure complexity.

**Not worth copying (but reference for Phase 2):**
- Mac Mini infrastructure with 47 agents — overkill for Phase 1, but the concept of specialized agents doing specific jobs is sound
- 8 MCP servers — we have 3 and that's sufficient to start
- Complex agent routing — oh-my-claudecode handles this in Phase 2
- The sheer scale — start with 2-3 terminals, not 47 agents

**TODO:** Harry needs to deeply think through his personal values and identity for the soul's identity half. This isn't something Claude can write for you — it needs to come from genuine self-reflection. The Cerebro "four values" pattern is a good template: pick 3-5 values that genuinely describe how you think and work.

---

## Phase 1: The Workshop

### Prerequisites

Before scaffolding jarvis, these tools must be installed and authenticated:

| Tool | Install | Why |
|------|---------|-----|
| **Claude Code** | `npm install -g @anthropic-ai/claude-code` | The engine |
| **GitHub CLI (`gh`)** | `brew install gh` then `gh auth login` | Creating private repos for workbenches, PRs, issue management |
| **Git** | Pre-installed on macOS (or `brew install git`) | Version control for jarvis root + all workbenches |
| **Node.js** | `brew install node` | Required for Claude Code, MCP servers, some plugins |
| **RTK** | `curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh \| sh` then `rtk init --global` | Token efficiency proxy — saves 60-90% of tokens per session |

**GitHub authentication is required** — the design assumes Claude can create GitHub remotes when scaffolding workbenches. Without `gh auth login`, this workflow breaks silently.

**RTK (Rust Token Killer)** is a single-binary CLI proxy ([rtk-ai/rtk](https://github.com/rtk-ai/rtk), 642 stars, MIT) that intercepts Bash commands and filters their output before it reaches Claude's context window. It uses four mechanisms: **smart filtering** (removes noise like comments/whitespace), **grouping** (aggregates similar items), **truncation** (preserves relevant context, cuts redundancy), and **deduplication** (collapses repeated lines with counts).

After running `rtk init --global`, it installs a `PreToolUse` hook that transparently rewrites commands — e.g., `git status` becomes `rtk git status`. Claude never sees the rewrite, it just gets optimized output. Supports: git operations (80-92% reduction), test runners (90%), directory listings (80%), linters, Docker, kubectl, npm/pip (70-85%).

**Why this matters for Jarvis:** Token efficiency is not just a cost issue — it determines session longevity (fewer `/clear` resets), context window headroom (more room for actual code), and multi-terminal viability (savings compound across all terminals). A typical 30-minute session drops from ~118K tokens to ~24K. Over a day with multiple terminals, this is the difference between hitting rate limits and running comfortably.

**Phase 2 additions:**
- GitHub Actions — for automated routines (daily scraper runs, unpushed commit checks, weekly reviews)
- Local LLM (Ollama) — for heartbeat/scheduler (see Phase 2 heartbeat section)

### Directory Structure

```
~/.claude/
├── CLAUDE.md                            ← THE SOUL (universal half)
│                                           Identity + operating principles. Loads in EVERY Claude session.
│                                           ~50-100 lines max.

~/jarvis/                                ← The hub (its own git repo)
├── .git/                                ← Tracks ideas, docs, shared — NOT workbenches
├── .gitignore                           ← Ignores workbench/*/ and projects/*/
├── CLAUDE.md                            ← THE SOUL (jarvis half)
│                                           Jarvis-specific workflows. Only loads inside ~/jarvis/.
│                                           ~50-100 lines max.
├── IDEAS.md                             ← Quick-capture scratchpad (dump ideas fast)
├── registry.json                        ← Machine-readable index of all ideas/projects
│                                           Updated incrementally. STATUS.md generated from this.
├── opportunities.md                     ← Claude's cross-project observations (triage queue)
│                                           Items added during work, processed during triage.
│                                           Self-cleaning — resolved items get removed.
│
├── ideas/                               ← LIBRARY 1: What to build — one file per idea
│   ├── _template.md                     ← Template for new idea files
│   ├── leetcode-for-ai.md
│   ├── reddit-scraper-digest.md
│   ├── personal-finance-dashboard.md
│   ├── learn-rust-by-building.md
│   └── ...
│
├── journal/                             ← LIBRARY 2: What we've learned — persists forever
│   ├── patterns.md                      ← What worked: reusable code, good decisions, winning approaches
│   └── antipatterns.md                  ← What failed: approaches that broke, libraries that caused issues, traps
│
├── memory/                              ← LIBRARY 3: What's happening now — rolling window
│   ├── STATUS.md                        ← On-demand dashboard (all projects, data flows, groups)
│   ├── 2026-02-10.md                    ← Daily memory note (auto-written at session end)
│   ├── 2026-02-11.md                    ← Read today + yesterday on session start
│   └── ...
│
├── workbench/                           ← Small/experimental projects (each has own .git)
│   ├── reddit-scraper/
│   │   ├── CLAUDE.md
│   │   ├── .git/
│   │   └── ...
│   ├── quick-experiment-xyz/
│   │   ├── CLAUDE.md
│   │   ├── .git/
│   │   └── ...
│   └── ...
│
├── projects/                            ← Serious projects (each has own .git)
│   ├── finance-dashboard/
│   │   ├── CLAUDE.md
│   │   ├── .git/
│   │   └── ...
│   ├── outreach-platform/
│   │   ├── CLAUDE.md
│   │   ├── .git/
│   │   └── ...
│   └── ...
│
├── shared/                              ← Inter-project data exchange
│   ├── README.md                        ← Conventions for how projects share data
│   ├── reddit-data/                     ← Output from reddit-scraper, consumed by others
│   │   ├── latest.json
│   │   └── _meta.json                   ← When updated, schema version, record count
│   ├── scraped-leads/                   ← Output from scraper, consumed by outreach project
│   │   ├── latest.json
│   │   └── _meta.json
│   └── ...
│
├── docs/
│   ├── plans/                           ← Design docs like this one
│   └── ref/                             ← Reference files (detailed rules extracted from soul)
│
└── (when soul files grow past ~100 lines, details get extracted into docs/ref/)
```

**Everything stays in jarvis by default.** Projects only leave jarvis on explicit request (rare). This ensures the jarvis soul always loads, giving Claude access to cross-project intelligence, memory, journal, and the idea file system regardless of which project you're working in.

The idea file (`~/jarvis/ideas/finance-dashboard.md`) has `workbench: projects/finance-dashboard` to link to the project inside jarvis.

### The Ideas Library (`ideas/`)

Each idea is a single markdown file created through conversation with Claude — you never have to write these by hand.

**Statuses:**

- `seed` — just captured, unrefined
- `exploring` — actively thinking about it, researching
- `ready` — designed enough to start building
- `active` — promoted to a workbench, being built
- `background` — running/maintained, not actively developing
- `paused` — on hold, might come back to it
- `archived` — done or abandoned

### How Ideas Get Created

You don't write idea files manually. You talk to Claude in `~/jarvis/` and Claude creates them. The flow:

**Step 1: You say you have an idea.**
Anything works — "new idea," "I've been thinking about," "what if we built," etc.

**Step 2: Claude asks what mode.**
- **Quick dump** — 2-3 questions, capture the gist, create a `seed` file. Done in 2 minutes. Good for when you have 5 ideas to get out of your head fast.
- **Brainstorm** — Full brainstorming skill. Explore the idea, clarify scope, discuss approaches. Creates a richer file with more detail, possibly `exploring` or `ready` status. Takes 10-15 minutes.

You pick in the moment based on how much energy you have for that idea.

**Step 3: Claude creates the file.**
Claude writes the idea file to `ideas/` with proper frontmatter, everything discussed, and the right status. You never touch the markdown directly unless you want to.

**Step 4: Revisiting ideas.**
When you come back later and say "let's look at that Reddit scraper idea," Claude reads the existing file and picks up where you left off. You can deepen a `seed` into a full brainstorm, change priority, or promote it to a workbench.

### Quick Capture (`IDEAS.md`)

For when you don't even want a conversation — just dump a one-liner. Like texting yourself a reminder:

```
- Chrome extension that highlights AI-generated text
- Tool that turns voice memos into structured meeting notes
- Something with flight price tracking??
```

Periodically, Claude helps you triage IDEAS.md — turn entries into proper idea conversations or discard them. Each one-liner becomes a "quick dump" or "brainstorm" depending on how you feel about it.

### Workbenches (`workbench/`)

When an idea graduates to "active," Claude scaffolds a project directory. Small experiments and early-stage projects go under `~/jarvis/workbench/`. When a project gets serious, it moves to `~/jarvis/projects/`. Both stay inside jarvis so the jarvis soul always loads. The distinction between workbench/ and projects/ is a TODO — figure it out through real usage.

Each workbench is an independent project with its own git repo, its own CLAUDE.md, and its own structure appropriate to what it is.

### The Shared Data Layer (`shared/`)

Projects need to talk to each other. The `shared/` directory is the convention for this:

**How it works:**
- A project that **produces** data writes to a named folder in `~/jarvis/shared/` (e.g., `shared/reddit-data/`)
- A project that **consumes** data reads from that folder
- Each shared folder has a predictable structure: `latest.json` (or `latest.csv`, etc.) for the most recent data, and must have dated archives

**Example pipeline:**
```
reddit-scraper (producer)
  → writes to ~/jarvis/shared/reddit-data/latest.json

outreach-platform (consumer)
  → reads from ~/jarvis/shared/reddit-data/latest.json
  → writes to ~/jarvis/shared/outreach-results/latest.json

analytics-dashboard (consumer)
  → reads from both shared/reddit-data/ and shared/outreach-results/
```

**Conventions:**
- **One writer per shared folder** — only one project writes to `shared/reddit-data/`. If two projects need to write similar data, they get separate folders. This prevents concurrent write conflicts, data corruption, and ambiguous ownership. This is a hard rule, not a suggestion.
- The producing project's CLAUDE.md documents what it writes and the schema
- The consuming project's CLAUDE.md documents what it reads and from where
- The idea file for each project lists its `produces:` and `consumes:` dependencies
- Every shared folder has a `_meta.json` with `producer`, `last_updated`, `schema_version`, and `record_count`
- `shared/README.md` is the master reference of all data flows

**Scope: any jarvis project.** `shared/` is for communication between any projects inside `~/jarvis/` — both `workbench/` and `projects/`. Since all projects live inside jarvis by default, shared/ is universally available. If a project ever leaves jarvis (rare, on explicit request), it must decouple from shared/ at that point.

**Why not APIs?** In Phase 1, file-based sharing is simpler — no servers to run, no ports to manage. In Phase 2, projects that need real-time communication can graduate to actual APIs, but the `shared/` convention covers 90% of workbench cases.

### Idea Capture From Anywhere

Ideas don't only happen at your desk. The system supports capture from three surfaces:

**1. Any Claude terminal (already designed):**
You're working in `~/jarvis/projects/finance-dashboard/` and have an idea. Say "new idea" — Claude creates the file in `~/jarvis/ideas/` without you switching terminals. Since everything is inside jarvis, the jarvis soul is always loaded.

**2. Quick text file (already designed):**
Open `~/jarvis/IDEAS.md` in any editor, dump a one-liner. Triage later.

**3. iMessage / Phone (Phase 2):**
Text or call Jarvis from your phone. Use cases:
- Walking and have an idea → text Jarvis a one-liner → it gets added to IDEAS.md or becomes a seed idea file
- Waiting somewhere → text "what's the status of the reddit scraper?" → get a summary back
- On a call and hear something relevant → text "look into X for the outreach project" → gets captured

**Implementation approach (design in Phase 2):**
- Apple Shortcuts automation that watches for messages to a specific contact ("Jarvis")
- Shortcut triggers a script on your Mac (via SSH, or local if Mac is awake)
- Script runs Claude in headless mode (`claude --print` or similar) with the message as input
- Claude reads the jarvis system, processes the request, writes the result
- Response sent back via iMessage through Shortcuts

This is a Phase 2 feature. Terminal + IDEAS.md covers 90% of idea capture. iMessage is high-value but it's a rabbit hole to build — don't let it distract from getting the core Workshop running. Build it when you're actually feeling the pain of not having it.

### Registry (`registry.json`)

The machine-readable backbone of the jarvis system. A single JSON file that's incrementally updated whenever an idea file changes — no full scans needed for routine queries.

```json
{
  "ideas": {
    "reddit-scraper": {
      "status": "active",
      "priority": "high",
      "category": "software",
      "group": "lead-gen-pipeline",
      "project_path": "projects/reddit-scraper",
      "produces": ["reddit-data"],
      "consumes": [],
      "last_touched": "2026-02-15",
      "created": "2026-02-10"
    },
    "finance-dashboard": {
      "status": "active",
      "priority": "medium",
      "category": "software",
      "group": null,
      "project_path": "projects/finance-dashboard",
      "produces": [],
      "consumes": [],
      "last_touched": "2026-02-12",
      "created": "2026-02-11"
    }
  },
  "groups": {
    "lead-gen-pipeline": ["reddit-scraper", "outreach-platform", "analytics-dashboard"]
  },
  "data_flows": {
    "reddit-data": { "producer": "reddit-scraper", "consumers": ["outreach-platform"] },
    "outreach-results": { "producer": "outreach-platform", "consumers": ["analytics-dashboard"] }
  }
}
```

**How it stays current:**
- When Claude creates or updates an idea file, it updates the corresponding entry in registry.json. One file write, not a full scan.
- When Claude scaffolds a workbench, it adds the `project_path` to the entry.
- When data flows change (produces/consumes), the `data_flows` section updates.
- The soul instruction: "When you modify an idea file, update registry.json to match."

**What it enables:**
- **Cheap status queries** — "What's active?" = read one JSON file, filter by status. One file read instead of 25.
- **Data dependency graph** — which projects produce/consume what, visible as structured data.
- **Group awareness** — related ideas are explicitly grouped, queryable without scanning.
- **Cross-project intelligence** — three-layer stack: registry.json (structural), journal/ (knowledge), opportunities.md (creative).

### Opportunities Queue (`opportunities.md`)

Claude's cross-project observations — creative connections, idea combinations, and "what if" insights that surface during work. This is NOT a permanent knowledge base — it's a triage queue. Items are added during work, processed during triage, and removed once resolved.

**What it looks like:**

```markdown
# Opportunities
> Claude's cross-project observations. Processed during triage.

- [ ] reddit-scraper's data fetching patterns (httpx + rotating UAs) could simplify finance-dashboard's Plaid API integration → **connects:** reddit-scraper, finance-dashboard
- [ ] outreach-platform's email template system is generic enough to extract as a shared utility → **connects:** outreach-platform, (any future project needing email)
- [ ] learn-rust-by-building could use reddit-scraper as the project to rebuild in Rust — real codebase, known behavior, easy to compare → **connects:** learn-rust-by-building, reddit-scraper
- [x] ~~reddit-scraper and outreach-platform should share rate limiting logic~~ → **resolved:** extracted to journal/patterns.md, both projects reference it
```

**How it works:**
- **During work:** When Claude notices a connection — an idea that could benefit from another project's approach, two ideas that combine into something better, a shared utility worth extracting — it adds a checkbox entry to opportunities.md.
- **During triage:** Harry processes each item. For each: act on it (create task, modify idea, adjust project), dismiss it (delete the line), or leave it for next triage.
- **Resolved items:** Struck through with a one-line resolution. Deleted next triage so Harry can see what was resolved.
- **Size signal:** If the file grows past ~20 unprocessed items, that means triage more often.

**The cross-project intelligence stack:**

| Layer | Type | Where | Lifecycle |
|-------|------|-------|-----------|
| Structural | Data flows, groups, metadata | registry.json | Permanent, updated incrementally |
| Knowledge | Reusable patterns, antipatterns | journal/ | Permanent, grows with experience |
| Creative | Opportunities, idea combinations | opportunities.md | Transient, resolved during triage |

**Why this replaced connections.md:** A "living intelligence document" duplicated data flows (already in registry) and patterns (already in journal), grew indefinitely, required a pre-work ritual, and went stale. The three-layer stack puts each type of intelligence where it naturally belongs, and adds opportunities.md only for the genuinely new thing: creative dot-connecting that gets processed and resolved, not accumulated forever.

### STATUS.md — The Dashboard View

`memory/STATUS.md` is a rendered view of the system state, generated primarily from `registry.json`. It gives Claude (and Harry) a human-readable snapshot of everything.

```markdown
# Jarvis Status
Generated: 2026-02-15 09:00

## Active Projects
- reddit-scraper (projects/) — last touched 2h ago
- outreach-platform (projects/) — last touched 1d ago
- finance-dashboard (projects/) — last touched 3d ago, blocked on API key

## Groups
### Lead Gen Pipeline
- reddit-scraper (active) → shared/reddit-data
- outreach-platform (active) → shared/outreach-results
- analytics-dashboard (seed) — not started yet

### Learning
- learn-rust-by-building (exploring)
- leetcode-for-ai (seed)

## Data Flows
reddit-scraper → shared/reddit-data (updated 2h ago, 847 records)
outreach-platform → shared/outreach-results (updated 1d ago, 23 records)

## Ideas Needing Attention
- 3 items in IDEAS.md not yet triaged
- 2 seed ideas older than 7 days (consider exploring or archiving)
```

**How it's generated:**
- **Default path (cheap):** Claude reads `registry.json` and formats it as markdown. One file read, fast.
- **Full scan (on demand):** When Harry explicitly asks for a deep status, or when the registry might be stale, Claude does a full scan — reads all idea files, checks git status on all projects, verifies shared/ data freshness. This also rebuilds/verifies registry.json against the actual files.
- **When it runs:** During triage (Claude offers), on explicit request ("status" / "what's going on"), or when Claude is already modifying idea files (marginal cost). NOT on every session start.

**The `group:` field in idea files:**
Ideas that are conceptually related share a group tag. Groups emerge organically — when Claude notices two ideas are related, it proposes grouping them. Groups appear in registry.json and STATUS.md and help Claude reason about the ecosystem as a whole.

### The Engineering Journal (`journal/`)

Claude's accumulated knowledge across all projects. Two files that grow over time:

**`patterns.md` — What Worked:**
```markdown
## API Design
- [2026-02-12] REST with versioned endpoints (/v1/) worked well for
  finance-dashboard. FastAPI auto-generates OpenAPI docs. Reuse this
  pattern for any project needing an API.

## Scraping
- [2026-02-15] Using httpx + BeautifulSoup with rotating user agents
  solved Reddit rate limiting. 3-second delay between requests is the
  sweet spot. Pattern in reddit-scraper/src/scraper.py.

## Testing
- [2026-02-18] For data pipeline projects, snapshot testing (save
  expected output, compare) catches regressions better than unit tests.
  Used in outreach-platform.
```

**`antipatterns.md` — What Failed and Why:**
```markdown
## Scraping
- [2026-02-13] Tried Selenium for Reddit scraping — overkill, slow,
  flaky. httpx is sufficient for static pages. Only use Selenium/
  Playwright when JavaScript rendering is required.

## Architecture
- [2026-02-16] Tried sharing a SQLite database between two projects
  (reddit-scraper and outreach). Locking issues when both write.
  Lesson: use the shared/ file convention instead. One writer per file.

## Dependencies
- [2026-02-19] chromadb 0.5.x has breaking changes from 0.4.x.
  Pin versions in requirements.txt, don't use >=.
```

**How Claude uses the journal:**
- Before making a technical decision, check if the journal has relevant patterns or warnings
- After discovering something (good or bad), add it to the journal with date and context
- Reference specific entries when making recommendations: "Based on what we learned on 2/13, let's use httpx instead of Selenium"
- Cross-project learning: a pattern proven in project A gets recommended for project B

**How journal files split and group (the grouping logic):**

Files start flat (`patterns.md`, `antipatterns.md`) with `## Domain` headers inside them. When a file hits ~100 lines, domains get extracted into their own files. The logic:

1. **Before adding a new entry**, check existing headers/files for a group that covers it. "API rate limiting" goes under `## API Design`, not a new `## Rate Limiting` section.
2. **When a file hits ~100 lines**, extract the largest domain section into its own file (e.g., `## Scraping` entries in `patterns.md` → `scraping-patterns.md`). The parent file gets a one-line reference: `See scraping-patterns.md for scraping patterns.`
3. **Domain files can split further** — if `scraping-patterns.md` hits ~100 lines and has clear sub-domains (Reddit vs generic web vs API scraping), split those into subsections or sub-files.
4. **Hierarchy stays shallow** — max two levels deep. `patterns.md` → `scraping-patterns.md` → stop. If a sub-file gets long, reorganize rather than nest deeper.
5. **Antipatterns follow the same pattern** — `antipatterns.md` → `scraping-antipatterns.md` when it gets long.

The goal: Claude should always know where to look without reading everything, and never create a new file when an existing one already covers the topic.

### Daily Memory (`memory/`)

The third library — ephemeral context that bridges sessions without bloating the soul.

**What lives here:**
- `STATUS.md` — the on-demand dashboard (moved here from jarvis root; it's context, not a permanent artifact)
- `YYYY-MM-DD.md` — daily memory notes, one per day

**Daily memory notes — what they contain:**
```markdown
# 2026-02-15

## What happened today
- Finished reddit-scraper auth flow, all tests passing
- Brainstormed outreach-platform idea, promoted to exploring
- Hit rate limit around 3pm, switched Terminal 2 to Sonnet

## Decisions made
- Using httpx over requests for scraping (added to journal)
- Reddit scraper will output to shared/reddit-data/ in JSON

## Open threads
- finance-dashboard blocked on Plaid API key (waiting on approval)
- Outreach idea needs competitor research before promoting to ready

## Tomorrow
- Triage IDEAS.md (3 items waiting)
- Continue outreach-platform brainstorm
- Check if Plaid API key came through
```

**When they're written:**
- At the end of a jarvis session, Claude writes/updates today's note automatically. Not a full transcript — a concise summary of what happened, what was decided, what's still open.
- No approval gate — Claude writes it and says "Memory note updated." If Harry sees something wrong next session, he corrects it then. An imperfect note that exists beats a perfect one that doesn't.

**When they're read:**
- On jarvis session start, Claude reads today's note (if it exists) + yesterday's note. This gives continuity without loading full conversation history.
- Two days is enough context. Older notes are there if you need them, but not auto-loaded.

**Lifecycle:**
- Daily notes accumulate but are low-cost (each is ~20-40 lines).
- Periodically archive old notes (30+ days) to a `memory/archive/` folder if the directory gets cluttered.
- Important decisions from daily notes should also land in the journal (permanent knowledge) — the daily note is the scratch pad, the journal is the textbook.

**How this differs from STATUS.md:**
- STATUS.md = state of all projects right now (what's active, what's blocked, data flows)
- Daily notes = narrative of what happened today (decisions, progress, open threads)
- STATUS.md is regenerated from scratch; daily notes are append-only within the day

### Claude Code Auto-Memory (built-in, separate from our system)

Claude Code has a built-in auto-memory feature that writes to `~/.claude/projects/<project>/memory/MEMORY.md`. This is **separate from and complementary to** our `~/jarvis/memory/` directory.

**What it is:**
- Claude proactively saves project-level notes: debugging gotchas, build quirks, code patterns, your preferences
- Per-project scope (one MEMORY.md per git repo)
- 200-line limit — first 200 lines injected into Claude's system prompt at every session start
- NOT in git — local to your machine only
- Can be disabled with `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1` (but we keep it ON)

**How it differs from our three libraries:**

| | Auto-memory (built-in) | Our `memory/` | Our `journal/` |
|---|---|---|---|
| **Scope** | Per-project | Cross-project | Cross-project |
| **Author** | Claude, proactively | Claude, auto-written (Harry corrects if needed) | Claude, with Harry's approval |
| **Content** | Project gotchas, build quirks | Daily narrative, STATUS dashboard | Permanent patterns/antipatterns |
| **Example** | "Tests need `--no-cache` flag" | "Worked on scraper, hit 403s" | "httpx with rotating UAs solves rate limiting" |

**Why we keep both:** Auto-memory handles the small per-project details ("this project's test suite needs X"). Our memory/ directory handles the big picture ("what happened today across all projects"). No overlap, no conflict.

**Periodic review:** The soul should instruct Claude to periodically review what auto-memory has saved (via `/memory` command) and flag anything that looks wrong or outdated. Weird auto-memory entries can cause unexpected behavior that's invisible if you only look at CLAUDE.md.

### The Soul — Two Files

The soul is split into two files that serve different scopes:

#### `~/.claude/CLAUDE.md` — Universal Principles (~50-100 lines max)

Loads in **every** Claude Code session. Contains only things that are always true, regardless of context. No jarvis-specific references.

```markdown
# CLAUDE.md

## Identity — Who Harry Is
- Builder and experimenter — learns by making things, not reading about them
- Ideas-first thinker — quantity of exploration matters more than premature polish
- Prefers seeing things work over perfect architecture
- Values: [TODO — Harry needs to define 3-5 personal values through self-reflection.
  Use Cerebro's "four values" pattern as a template. These should genuinely describe
  how you think and work, not aspirational platitudes.]

## Operating Principles — How Claude Works

### Decision-Making
- Always plan before coding (use superpowers)
- Always verify before claiming done
- When unsure, ask — don't guess
- Keep things simple until complexity is earned
- Use TDD for anything that ships; skip for experiments
- If you think I'm wrong, say so. You have permission to disagree and push back.
- Don't let my enthusiasm override your judgment. If I'm excited about a bad idea, it's your job to slow me down.

### Model Routing
- Main session model is YOUR choice at session start. Default: Sonnet. Switch to Opus via `/model` when the task needs it.
- For medium-weight delegated tasks (triage summaries, status compilation, formatting), spawn sub-agents on fast models via Task tool rather than doing it on the main session.
- Don't bother delegating trivial tasks — sub-agent startup overhead isn't worth it for quick checks.
- When in doubt, do it yourself on the current model. Only delegate when the savings are clear.

### Context Management
- When context is getting long and important info is captured in files, suggest compacting
- ALWAYS ask before clearing or compacting — never auto-clear
- The file IS the memory — once something is written to disk, the conversation is disposable
- When approaching rate limits, say so and suggest switching models or pausing
- Auto-memory is active. Periodically review what's saved (via `/memory`) and flag anything odd or outdated.

### Communication
- Be direct. No fluff, no filler, no performative agreement.
- When presenting options, lead with your recommendation
- Don't over-explain things I already know
- Flag risks early, don't bury them
- Don't pad responses to seem thorough. Short and right beats long and padded.

### When Things Go Wrong
- If you've attempted the same fix twice and it's not working, stop. Say "I'm going in circles. Let me rethink."
- If the context feels contradictory (soul says X, project CLAUDE.md says Y), flag the conflict instead of guessing.
- If I seem frustrated, acknowledge it directly. Don't pretend everything is fine.

### Boundaries
- Autopilot may execute bounded tasks; it may not change goals, roadmap, or product direction without approval.
- Never modify soul files (~/.claude/CLAUDE.md or ~/jarvis/CLAUDE.md) without proposing the change and getting approval
- Never delete idea files — archive them instead
- Never write to shared/ folders this project doesn't own (check _meta.json for owner)

### Soul Evolution
- If you notice a pattern in how Harry works that isn't captured here, propose adding it
- Don't edit silently — suggest the change and explain why
- This file must stay under ~100 lines. Extract details into reference files in the project's docs/ref/.
```

#### `~/jarvis/CLAUDE.md` — Jarvis System (~50-100 lines max)

Loads **only** when Claude is running inside `~/jarvis/` or its subdirectories. Contains all jarvis-specific workflows.

```markdown
# Jarvis System

## Three Libraries
- ideas/ — what to build (one file per idea, created through conversation)
- journal/ — what we've learned (patterns + antipatterns, split by domain when long)
- memory/ — what's happening now (STATUS.md + daily notes)

## Structured Data
- registry.json — machine-readable index of all ideas/projects. Update incrementally when idea files change.
- opportunities.md — Claude's cross-project observations. Add during work, Harry processes during triage.

## The System
- Active projects live in ~/jarvis/workbench/ (experimental) or ~/jarvis/projects/ (serious)
- Workbench-to-workbench data flows through ~/jarvis/shared/ (one writer per folder)
- Every project has a corresponding idea file AND registry entry — always keep both in sync

## Session Initialization

**Loads (always, automatic):**
- `~/.claude/CLAUDE.md` — universal soul (auto-loaded by Claude Code)
- Current project's `CLAUDE.md` — if present (auto-loaded by Claude Code)
- `~/jarvis/CLAUDE.md` — jarvis soul, only when working inside `~/jarvis/` (auto-loaded)

**Memory load (default, cheap):**
- Read `~/jarvis/memory/YYYY-MM-DD.md` (today)
- If today's file doesn't exist, read yesterday's (only one day back)
- Do NOT read older memory/history unless explicitly requested

**Status load (only when needed):**
- If user says "triage," "what's going on," "status" → read registry.json (cheap), generate STATUS.md from it
- For deep status (explicit request): full scan of idea files + git status to verify/rebuild registry
- Otherwise skip status entirely by default

**Model routing:**
- Session model is Harry's choice. Default Sonnet, Opus when needed.
- For medium-weight tasks (status compilation, formatting, triage summaries), delegate to sub-agents on fast models via Task tool.
- Don't delegate trivial tasks — sub-agent overhead isn't worth it for quick checks.

## Idea Workflow
- Harry says "new idea"? Ask: quick dump or brainstorm? Then create the idea file
- Harry wants to revisit an idea? Read the existing file, pick up where we left off
- Never make Harry write markdown by hand — that's Claude's job
- Ideas grow progressively: seed (minimal) → exploring (add research) → ready (add architecture, tech stack, MVP scope)

## Session Workflow
- Triage session? Read registry.json, offer to regenerate memory/STATUS.md from it, check IDEAS.md for anything to triage, present opportunities.md for processing
- Working on a workbench? Read its CLAUDE.md first
- Finishing something? Update the idea file's status and last_touched, push if there's a remote
- Ending a session? Auto-write memory/YYYY-MM-DD.md — summarize what happened, decisions made, open threads. No approval gate. Say "Memory note updated." Harry corrects next session if needed.
- Every 2 weeks, review memory/ notes for recurring manual tasks that could be automated.
- Scaffolding a new workbench? Create git repo, offer to create a GitHub remote (default yes)
- Never let a workbench exist without a remote for more than a week — remind Harry

## Cross-Project Thinking
- During work: when you notice a creative connection (idea A could benefit from idea B, a shared utility worth extracting, two ideas that combine into something better), add a checkbox entry to ~/jarvis/opportunities.md
- During triage: present opportunities.md items for Harry to process (act, dismiss, or defer)
- Structural connections (data flows, groups) live in registry.json — don't duplicate in opportunities.md
- Knowledge connections (reusable patterns) live in journal/ — don't duplicate in opportunities.md
- When modifying an idea file, update registry.json to match

## Token Efficiency
- Do NOT run `git diff` before committing — it floods context with token-heavy diffs (10,000-50,000 tokens). Use `git status` + `git diff --stat` instead (~100-500 tokens).
- When you need specific file changes, use `git diff -- <file>` selectively, not the whole diff.
- Don't read entire files when you only need a section — use Grep first, then Read with offset/limit.
- Don't re-read files already read this session unless they've been modified.
- Prefer Grep and Glob over Bash find/cat/grep commands.
- RTK is installed. It handles command output filtering automatically. Trust its output.

## Health Checks (during triage)
- Zombie projects: active status but no commits in 2+ weeks? Suggest pausing.
- Unpushed work: local commits without a remote? Remind to push.
- Stale shared data: _meta.json last_updated > 7 days but consumer is active? Flag it.
- IDEAS.md backlog > 10 items? Nudge toward triage.
- Auto-memory: spot-check /memory for active projects — flag stale or wrong entries.
- Soul drift: doing something repeatedly that the soul doesn't mention? Propose adding it.
- These are suggestions, not gates. Harry decides what to act on.

## Kill / Archive Cadence (anti-entropy)

**Weekly triage — flag:**
- Any idea in `seed` or `exploring` not touched in 30 days
- Any active workbench/project not touched in 14 days (zombie check)

**Forced decision for each flagged item:**
- **Archive** (default) — not deletion, just mark `status: archived`, `archived_reason: <1 line>`, `last_touched: YYYY-MM-DD`
- **Pause** — requires a next-review date
- **Promote** — define next action + scope

**Why:** This keeps the library lean and prevents jarvis from becoming a graveyard of abandoned ideas.

## Engineering Journal
- Only add to journal/ when a lesson generalizes across future projects. Project-specific stuff stays in memory/.
- Before technical decisions, check relevant journal files for patterns or warnings
- After learning something (good or bad), add it to the appropriate journal file
- Before creating a new domain file, check if an existing group already covers the topic
- Split by domain (scraping.md, api-design.md, etc.) when a file hits ~100 lines
- This file must stay under ~100 lines. Extract details into docs/ref/ if needed.
```

(Both drafts — refine after a week of real usage.)

---

## Idea File — Progressive Template

Idea files grow with the idea. Claude adds sections through conversation as the status progresses.

### Seed (quick dump — the starting point)

```markdown
---
title: [Idea Name]
status: seed
category: [software | experiment | automation | content | business | learning]
created: YYYY-MM-DD
last_touched: YYYY-MM-DD
priority: [high | medium | low | someday]
group: null
workbench: null
produces: []
consumes: []
---

# [Idea Name]

## What
[One paragraph: what is this idea?]

## Why
[Why does this matter to you? What problem does it solve?]

## Notes
[Anything else — links, inspirations, constraints, open questions]
```

### Exploring (added during brainstorm / research)

Claude adds these sections when the idea moves to `exploring`:

```markdown
## Success Looks Like
[How would you know this worked?]

## Research
[What we've learned so far — links, findings, competitive landscape]

## Open Questions
[Things we still need to figure out before building]
```

### Ready (added when the idea is designed enough to build)

Claude adds these sections when promoting to `ready`:

```markdown
## Architecture
[High-level approach — what components, how they connect]

## Tech Stack
[Languages, frameworks, services, APIs]

## MVP Scope
[What's the smallest version that delivers value? What's explicitly NOT in v1?]

## Risks
[What could go wrong? What are we unsure about?]
```

### Active (added when workbench is created)

```markdown
## Workbench
[Path to the project directory]
[Link to GitHub remote if it exists]
```

**Field notes:**
- `workbench:` starts as `null`, becomes `workbench/project-name` or `~/projects/project-name` when activated
- `group:` optional tag for related ideas (e.g., `"lead-gen-pipeline"`)
- `produces:` list of `shared/` folders this project writes to (e.g., `["reddit-data"]`)
- `consumes:` list of `shared/` folders this project reads from (e.g., `["reddit-data", "scraped-leads"]`)

---

## Cursor Integration

Cursor and Claude Code serve different roles in the same system:

| Role | Tool | When |
|------|------|------|
| **Engine** | Claude Code (terminal) | Planning, scaffolding, multi-file changes, autonomous work, running tests, git operations |
| **Cockpit** | Cursor (IDE) | Reviewing diffs, manual edits, visual inspection, debugging with breakpoints, reading code you don't understand yet |

### How They Coexist

**Same files, different interfaces.** Both Claude Code and Cursor operate on the same directory trees. No sync issue — they read/write the same files on disk.

**Cursor workspace setup:**
- Open `~/jarvis/` as a Cursor workspace for the hub (ideas, docs, shared data)
- Open individual project directories (workbench/ or projects/) in Cursor when doing hands-on work
- Cursor's AI features (Cmd+K, inline edit) work on the same files Claude Code touches

**Workflow example:**
1. In terminal: `cd ~/jarvis/workbench/reddit-scraper && claude`
2. Claude builds the scraper, runs tests, commits
3. You open Cursor, navigate to the file, review what Claude wrote
4. You make a manual tweak in Cursor (surgical edit)
5. Back in terminal, Claude picks up where you left off — it sees your edit

**Cursor rules (`.cursorrules`):**
Each project can optionally have a `.cursorrules` file for Cursor-specific AI behavior. Separate from CLAUDE.md — Cursor reads `.cursorrules`, Claude Code reads `CLAUDE.md`. They can contain different instructions tailored to each tool's strengths.

---

## Multi-Terminal Workflow

### Daily Pattern

```
Terminal 1: cd ~/jarvis && claude
  → "Triage mode" — review IDEAS.md, check status across projects,
    decide what to work on today

Terminal 2: cd ~/jarvis/workbench/project-a && claude
  → Deep work on Project A

Terminal 3: cd ~/jarvis/projects/project-b && claude
  → Deep work on Project B (serious project, still inside jarvis)
```

### Context Hygiene

The hub terminal handles a lot — triage, brainstorming, status checks, idea capture. Strategy:
- **Quick operations** (status, quick dumps, triage) are lightweight — do several per session
- **Brainstorms** are heavy — after a full brainstorm, the idea file captures everything. Claude suggests compacting but ALWAYS asks first: "The idea is saved in the file. Want me to compact the conversation?"
- **Never auto-clear** — Harry must approve every clear/compact. The soul enforces this.
- **The file IS the memory** — once an idea, journal entry, or status update is written to disk, it's safe. The conversation is disposable; the files are permanent.

### Terminal Allocation Guidelines

| Scenario | Terminals | Why |
|----------|-----------|-----|
| Exploring a new idea | 1 (in `~/jarvis/`) | You're brainstorming, not building yet |
| Building one project | 1 (in its workbench) | Focused deep work |
| Building two projects in parallel | 2 (one per workbench) | Isolated contexts, no conflicts |
| Building + running experiments | 2-3 | One for the build, one for experiments |
| Triage + build + monitor | 3 | Dashboard terminal + work terminals |
| Pipeline work (producer + consumer) | 2 | One terminal per project in the pipeline |

### Rate Limit Awareness

All terminals share your subscription quota. Strategy:
- **Terminal 1 (triage/light):** Use Haiku or Sonnet — cheap, fast, good for status checks
- **Terminal 2-3 (deep work):** Use Opus — complex reasoning, architecture, multi-file changes
- Check `/usage` periodically
- When approaching limits, Claude should proactively say so and suggest switching to a lighter model or pausing one terminal

### Budget Awareness

Running multiple Opus terminals burns through Claude Pro limits fast. The soul should include cost-consciousness:
- **Default to Sonnet** unless the task genuinely needs Opus-level reasoning (architecture, complex debugging, multi-file refactors)
- **Haiku for throwaway work** — quick checks, formatting, simple file generation
- **Opus for the hard stuff** — planning, system design, complex code generation, cross-project reasoning
- **Track daily spend** — run `/usage` at the start and end of each day. If you're consistently hitting limits, it means too many terminals are running Opus simultaneously.
- In Phase 2, add a cost log to `~/jarvis/logs/` so spending is tracked over time.

### Backups

Workbenches that haven't been pushed to a remote can be lost (disk failure, accidental deletion). Strategy:
- **When Claude scaffolds a new workbench**, it creates a git repo and offers to create a GitHub remote (private repo). This is the default — opt out, not opt in.
- **The soul includes a reminder**: when finishing a work session, push if there's a remote.
- In Phase 2, add a weekly routine that checks all workbenches for unpushed commits and flags them.

### STATUS.md — Keeping It Fresh

`memory/STATUS.md` is a rendered view of `registry.json`, not a primary data source. It's regenerated on-demand:
- **During triage**: Claude reads `registry.json` and renders STATUS.md from it. Cheap — one file read + formatting.
- **On explicit request**: "Update status" or "what's the state of everything" triggers regeneration from registry.
- **Deep refresh (on explicit request)**: When Harry asks for a thorough status or when the registry might be stale, Claude does a full scan — reads all idea files, checks git status on all projects, verifies shared/ data freshness. This also rebuilds/verifies registry.json against actual files.
- **NOT on every session start**: Even with registry making it cheap, skip unless it's a triage session.
- **Cost at scale**: Default path (read registry.json) is O(1) regardless of how many ideas exist. Full scan path is O(n) but only fires on explicit request.

### System Health Checks

During triage sessions, Claude runs through these checks and flags anything that needs attention. Not a formal monitoring system — just a checklist that prevents slow drift.

**Project Health:**
- **Zombie active projects** — idea file says `status: active` but the workbench/project hasn't had a git commit in 2+ weeks. Suggest pausing or archiving.
- **Unpushed work** — a workbench or project has local commits not pushed to a remote, or has no remote at all. Remind Harry to push or create a remote.
- **Stale shared data** — a `shared/` folder's `_meta.json` shows `last_updated` older than 7 days but a consuming project is still active. Flag the staleness.

**Idea Health:**
- **IDEAS.md backlog** — more than 10 unprocessed items in the quick-capture file. Nudge toward a triage session.
- **Orphaned ideas** — idea files with `status: active` but no corresponding workbench/project directory. Something got out of sync.

**Memory Health:**
- **Stale daily memory** — memory notes older than 30 days not archived to `memory/archive/`. Suggest archiving.
- **Auto-memory drift** — during triage, spot-check auto-memory entries (`/memory`) for active workbenches. Flag anything stale (references old tools/versions), wrong, or contradicting the journal.

**Knowledge Health:**
- **Soul drift** — if Claude notices it's repeatedly doing something the soul doesn't describe (or contradicts), flag it. "I've been doing X in the last 3 sessions but the soul doesn't mention it. Should we add it?"
- **Journal relevance** — when checking journal files, flag entries that reference tools/libraries no longer used in any active project. Stale patterns are misleading patterns.

**When these run:**
- Claude runs the full checklist during triage sessions (when you're in `~/jarvis/` doing status checks)
- Individual checks fire opportunistically — e.g., unpushed work gets flagged at session end, zombie projects when reading STATUS.md
- This is NOT a blocking gate — Claude flags issues as suggestions, Harry decides what to act on

---

## Phase 2: The Studio (Growth Path)

> Don't build this yet. This section documents what gets added WHEN Phase 1 feels limiting.

### What Gets Added

```
~/jarvis/
├── ... (everything from Phase 1, including registry.json and opportunities.md) ...
│
├── pipelines.json                   ← Map of data flows between projects
├── routines/                        ← Scheduled/recurring tasks
│   ├── daily-digest.md              ← Morning summary of all project status
│   ├── reddit-scrape.md             ← Daily Reddit data collection
│   └── weekly-review.md             ← Weekly review prompt
├── logs/                            ← Execution logs from autonomous runs
│   ├── 2026-02-10-reddit-scraper.log
│   └── ...
└── scripts/
    ├── dispatch.sh                  ← Launch Claude instances for scheduled routines
    ├── morning.sh                   ← "Open laptop" script — run daily routines
    ├── status.sh                    ← Quick status across all workbenches
    └── pipeline.sh                  ← Run a multi-project pipeline end-to-end
```

### Pipelines (`pipelines.json`)

Formal definition of data flows between projects:
```json
{
  "reddit-to-outreach": {
    "steps": [
      { "project": "reddit-scraper", "action": "scrape", "output": "shared/reddit-data" },
      { "project": "outreach-platform", "action": "process-leads", "input": "shared/reddit-data", "output": "shared/outreach-results" }
    ],
    "schedule": "daily",
    "notify_on_failure": true
  }
}
```

### Routines (`routines/`)

Things that should happen on a schedule. Each routine is a markdown file describing:
- What to do
- When to run (daily, weekly, on-demand)
- Which workbench it operates on
- What to report back

### Morning Script (`scripts/morning.sh`)

Run when you open your laptop:
1. Checks all active workbenches for status (last commit, test results, open TODOs)
2. Runs any daily routines
3. Executes scheduled pipelines
4. Generates a digest to your terminal or a `logs/daily-digest-YYYY-MM-DD.md`

### Orchestration

When you have 5+ active workbenches, add oh-my-claudecode for:
- Smart model routing (Haiku for simple, Opus for complex)
- Magic keywords ("autopilot: finish the reddit scraper tests")
- Coordinated multi-agent execution
- Pipeline orchestration across multiple Claude instances

### Heartbeat / Scheduled Agent Check-ins

Use a **local LLM** (Ollama, llama.cpp, or similar) as the scheduler/heartbeat — NOT Claude API. The local model's job is lightweight:
- Wake up on schedule (cron, launchd, or similar)
- Check: does any routine need to run? Are any workbenches stale? Is STATUS.md outdated?
- If work is needed: spin up a Claude Code instance (`claude --print` or headless mode) with the specific task
- If no work needed: log "heartbeat: nothing to do" and go back to sleep

**Why local LLM, not Claude API for scheduling:**
- Zero token cost for heartbeats that find nothing to do (90% of checks)
- No rate limit consumption for routine checks
- Local model is fast enough for "should I wake Claude up?" decisions
- Claude API is reserved for actual work, not scheduling logic

**Implementation (design when Phase 2 starts):**
- A small script with a local model that runs every N hours
- Checks a `routines/` manifest for what should run and when
- Launches Claude Code instances for actual work
- Logs all activity to `logs/`

### Rate Limit Guardrails

**TODO — needs real usage data to finalize, but current thinking:**
- Don't let Claude go on long autonomous loops without check-ins. If a task runs 20+ tool calls without human feedback, pause and summarize progress.
- Daily budget awareness: run `/usage` at start and end of each day. Track trends.
- When multiple terminals are running Opus, proactively suggest downgrading one to Sonnet.
- Phase 2: formalize into a daily token budget with logging.

### Two-Layer Review System

Code and architecture review in Jarvis happens in two layers. Layer 1 is Claude reviewing itself. Layer 2 is an external model providing a genuinely different perspective. Layer 2 is an escalation — it only fires after Layer 1, or when you explicitly ask.

#### Layer 1: Claude's Internal Review (Self-Review)

Already partially available in Phase 1, fully enabled in Phase 2:

**Phase 1 (available now):**
- `/superpowers:requesting-code-review` — spins up a Claude sub-agent to review code quality, patterns, bugs. Works today with the superpowers plugin.
- `/superpowers:verification-before-completion` — forces Claude to prove something works before claiming it's done. Prevents "it should work" without evidence.

**Phase 2 (with Agent Teams):**
- Spawn 3-5 Claude Agent Team members with different roles: one plays devil's advocate, one focuses on security, one checks architecture consistency. They debate via `sendMessage`, try to break each other's assumptions, and converge on findings.
- This is Claude reviewing itself from multiple angles — different prompts/roles, same model. Catches the majority of issues.

**When Layer 1 is sufficient:** Most coding tasks, refactors, bug fixes, standard architecture decisions. Claude's own review with adversarial agents handles 90% of cases.

#### Layer 2: External Multi-Model Review (Escalation)

A custom MCP server (`jarvis-review`) that wraps one or more external model APIs, giving Claude a `second_opinion` tool. This provides a genuinely different perspective — different training data, different biases, different blind spots from Claude's.

**Architecture:**
- MCP server wraps model APIs behind a single `second_opinion(code, question, context)` tool
- Model-agnostic design: adding a new model = config change, not a rewrite
- Start with the cheapest viable option, add more as needed

**Candidate models (start with one, design for many):**
- **Google Gemini** — generous free tier (1500 req/day on 2.0 Flash), 1M context window, cheapest to start
- **DeepSeek API** — $0.14/M input tokens, strong reasoning, very different training biases (Chinese-developed)
- **OpenAI GPT-4o** — strongest alternative code reviewer, most different architecture from Claude. Requires subscription.

**When Layer 2 fires:**
- **On explicit request:** You say "get a second opinion on this"
- **Auto-suggested by Claude (TODO — triggers need refinement after real usage):**
  - Architecture decisions (database schema, API design, auth flow)
  - Security-sensitive code (auth, encryption, user data handling)
  - When Claude is uncertain (hedging, presenting multiple options without strong recommendation)
- **Never fires without Layer 1 happening first**

**What it is NOT:**
- Not a multi-round debate system (CodeMoot-style) — it's a single-shot "here's the code/question, what do you think?"
- Not autonomous — Claude proposes getting a second opinion, you approve (unless it's an auto-trigger)
- Not a replacement for Claude's own review — it's an escalation for high-stakes or blind-spot situations

**TODO (revisit after using Layer 1 for a while):**
- Finalize auto-trigger list — which situations genuinely benefit from Layer 2 vs just being noisy?
- Decide first model to wire up based on cost/quality testing
- Design the MCP server (small project, likely a single workbench)
- Determine how Claude should present the external model's feedback (raw? synthesized? side-by-side?)

### Agent Teams (Built-in Claude Code Feature)

Claude Code shipped Agent Teams as an experimental feature — a different execution model from regular sub-agents where 3-5 independent Claude Code instances actually collaborate on the same project.

**Old sub-agent model:** Main agent → Task tool → sub-agent works in isolation → session terminates → only a summary comes back.

**New Agent Teams model:** Shared task lists, direct messaging between agents, explicit lifecycle control (startup, shutdown). Agents can coordinate, debate, and update each other in real time instead of working in silos.

**Internal tools (5 new):**
- `TeamCreate` — sets up team scaffolding (creates `.claude/teams/` folder)
- `TaskCreate` — adds team-scoped tasks as JSON files with status, dependencies, ownership
- `Task` (upgraded) — now supports `name` and `team_name` params to activate team mode
- `taskUpdate` — agents claim tasks, update status, mark done
- `sendMessage` — direct messages (agent→agent) and broadcasts (agent→all). Messages written to `.claude/teams/<team_id>/inbox/` and injected into each agent's conversation history

**Best use case:** Deep debugging with multiple hypotheses. Spawn 5 agents to investigate different theories. They talk to each other, try to disprove each other's ideas (scientific debate style), and update a findings doc with consensus.

**How it stacks with our plan:**
1. **Agent Teams** = the engine (built-in primitives). Enable flag in Phase 1.
2. **oh-my-claudecode** = the wrapper (magic keywords, HUD, smart routing). Add in Phase 2.
3. **wshobson/agents** = the templates (pre-built specialist teams). Cherry-pick in Phase 2.

**Setup:** Add `"CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"` to `~/.claude/settings.json` env. Use tmux/iTerm2 for parallel pane visibility (`claude --teammate-mode tmux`).

### Triggers for Moving to Phase 2

You'll know it's time when:
- You have 4+ active workbenches and lose track of status
- You keep wanting something to "just run every morning"
- You find yourself manually checking project status across terminals
- You wish Claude remembered what happened yesterday across projects
- Your `shared/` folder has 5+ data flows and you need to visualize dependencies

---

## Open TODOs

Items flagged for future design sessions:

- [ ] **Harry's personal values for soul** — The identity half of the soul needs 3-5 genuine personal values (Cerebro's "four values" pattern). This requires real self-reflection, not Claude writing it. Use the template in the soul draft and replace the TODO block.
- [ ] **Project CLAUDE.md template** — Draft exists conceptually but needs dedicated brainstorming on what it should include, look like, and how it connects back to the jarvis system. Do NOT finalize until this session happens. This is high priority — it's what makes each project "jarvis-aware."
- [ ] **Workbench vs projects distinction** — Both live inside `~/jarvis/`. What makes a project "serious" enough to move from `workbench/` to `projects/`? Quality gate? Scale? Complexity? Just organization? Figure out through real usage.
- [ ] **Rate limit guardrails** — Need real usage data before finalizing. Current thinking: 20+ tool call pause rule, daily `/usage` tracking, proactive model downgrade suggestions. Formalize after 1-2 weeks of real multi-terminal usage.
- [ ] **Soul refinement** — Current drafts are starting points. Refine both files after 1 week of real usage based on observed patterns.

### Resolved

- [x] **Soul symlink flaw** — Fixed: split into `~/.claude/CLAUDE.md` (universal, ~100 lines max) + `~/jarvis/CLAUDE.md` (jarvis-specific, ~100 lines max). Jarvis rules only load in jarvis context. Caught by Cursor review 2.
- [x] **Journal scalability** — Fixed: start with flat files, split by domain when a file hits ~100 lines. Directory structure supports this from day one. Claude reads only relevant files.
- [x] **STATUS.md regeneration** — Fixed: on-demand, not automatic. Claude offers during triage, auto-regens only when already modifying idea files. Avoids latency tax.
- [x] **Shared data coupling** — Resolved differently: all projects stay inside jarvis by default, so shared/ is available to all. Decoupling only needed if a project explicitly leaves jarvis (rare).
- [x] **Soul size budget** — Fixed: ~100 lines max per file. Extract details into reference files when growing.
- [x] **Idea template growth** — Fixed: progressive sections that grow with status (seed → exploring → ready → active).
- [x] **iMessage integration** — Moved to Phase 2. Terminal + IDEAS.md covers 90% of capture needs.
- [x] **Budget tracking** — Resolved: model selection strategy in soul + daily `/usage` checks + Phase 2 cost logging.
- [x] **Backups** — Resolved: default to creating GitHub remotes on scaffold, soul reminds to push, Phase 2 weekly check routine.
- [x] **Shared data write safety** — Resolved: "one writer per shared folder" as hard rule, `_meta.json` for every shared dataset.

---

## Next Steps

### Phase 1 Build Order

1. **Install prerequisites** — gh CLI, RTK (`rtk init --global`), enable Agent Teams flag (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in `~/.claude/settings.json` env)
2. **Harry's values** — Self-reflect on 3-5 personal values for the soul identity half (not a Claude task)
3. **Brain dump** — Get all 10-25 ideas out of Harry's head into idea files (conversational)
4. **Design project CLAUDE.md template** — Brainstorm session (flagged TODO, high priority)
5. **Scaffold `~/jarvis/`** — Create the directory structure: ideas/, journal/, memory/, workbench/, projects/, shared/, docs/ref/, .gitignore, templates, both CLAUDE.md soul files, registry.json (empty `{}`), opportunities.md (empty checklist)
6. **Write the soul** — Finalize both `~/.claude/CLAUDE.md` and `~/jarvis/CLAUDE.md` with real values, sub-agent model delegation rules, session init rules, token efficiency rules, opportunities.md workflow
7. **Promote 1-2 ideas** — Graduate the most exciting ones to workbenches. Registry.json starts populating naturally from here.
8. **Work in the system** — Use it for a week, note what's missing. Daily memory notes will start accumulating.
9. **Iterate** — Adjust structure, soul, and conventions based on real usage
10. **Resolve graduation process** — Once a project actually needs to move, design the process from real experience

### Phase 2 (When Triggered)

> There is no Phase 3. Everything beyond Phase 1 lives here. Grouped by urgency.

#### Tier A — Core (add when Phase 1 creaks)

These are the upgrades that unlock Phase 2's value. Do these first.

11. **Orchestration** — Add oh-my-claudecode, wshobson/agents (cherry-pick specialists), routines, morning script
12. **Agent Teams workflows** — Built-in Claude Code feature for multi-agent coordination. The flag is already enabled (Phase 1 step 1). Now build workflows: spawn investigation teams for complex debugging, architecture review teams, security audit teams. Three layers stack: Agent Teams (engine, built-in) → oh-my-claudecode swarm (wrapper, adds magic keywords + HUD + smart routing) → wshobson/agents agent-teams plugin (pre-built team templates). Setup: use tmux or iTerm2 for parallel pane visibility (`claude --teammate-mode tmux` opens one pane per agent). **Important: start with 2-agent teams on a non-critical task to validate the primitives work as described before building 5-agent workflows. Agent Teams is an experimental flag — the described coordination patterns (sendMessage debates, shared task lists) may be buggy, slow, or token-expensive in practice.**
13. **Multi-model review MCP server (`jarvis-review`)** — Layer 2 of the two-layer review system. Build a small MCP server that wraps external model APIs behind a `second_opinion(code, question, context)` tool. Start with cheapest viable model (likely Gemini free tier or DeepSeek), design so adding models is a config change. Only fires after Claude's own Layer 1 review (superpowers + Agent Teams) has already happened. **TODO: finalize auto-trigger list and first model choice after real Layer 1 usage.**
14. **Heartbeat system** — Local LLM scheduler for routine checks and agent wake-ups

#### Tier B — Pain-Driven (add when specific pain emerges)

These solve real problems but only matter once you're hitting them.

15. **Rate limit guardrails** — Formalize daily budgets, pause rules, model routing enforcement
16. **Cost logging** — Track spend over time in ~/jarvis/logs/
17. **Pipeline formalization** — pipelines.json for complex multi-project data flows
18. **claude-agent-borg** — Assimilate useful patterns from other Claude Code setups (CLAUDE.md files, hooks, skills) intelligently. Evaluate and integrate rather than blindly copy. ([repo](https://github.com/davidpp/claude-agent-borg))

#### Tier C — Nice to Have (add when you want them)

Genuinely useful but not blocking anything. Install when the mood strikes.

19. **Voice interface** — claude-code-voice-skill enables phone conversations with Claude about your codebase via Vapi ($2/month). Best use: brainstorming while walking, quick status checks across projects, hands-free triage. Install: `pip install claude-code-voice` → `claude-code-voice setup` → register projects. ([repo](https://github.com/abracadabra50/claude-code-voice-skill))
20. **iMessage integration** — Design and build mobile capture
21. **WebMCP (watch)** — W3C standard (Chrome 146 Canary) letting websites register structured tools for AI agents to call directly instead of screenshot-based automation. Currently behind `chrome://flags` → "WebMCP for testing." No websites have adopted it yet. When it matures (est. late 2026), it could augment or partially replace chrome-devtools MCP for web-facing workflows. Don't build around it — just watch. ([Chrome blog](https://developer.chrome.com/blog/webmcp-epp)) ([W3C spec](https://webmcp.link/))
