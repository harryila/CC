# Claude Code – Analysis & Stack

Analysis of 25 GitHub repos for building Claude Code infrastructure: safety, overlaps, recommended stack, and phased install plan.

**Main doc:** [MASTER_ANALYSIS.md](./MASTER_ANALYSIS.md)

The 25 repos were cloned locally for analysis; they are not included in this repo (see their upstream GitHub repos). This repo contains only the analysis output and recommendations.

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

---

## Design Decisions Log

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Project location | **Hybrid** — small experiments in `~/jarvis/workbench/`, serious projects get their own location (e.g., `~/projects/`) | Keeps jarvis from becoming a junk drawer, but small stuff doesn't need top-level dirs |
| Git strategy | **Jarvis root is a repo, workbenches are gitignored** — each workbench/project has its own git repo | Ideas/docs are versioned together; project code doesn't pollute jarvis history |
| Project linking | **Path in idea file** — the idea file's `workbench:` field stores the path to the project wherever it lives | Single source of truth, no symlinks to maintain |
| Soul scope | **Split** — `~/.claude/CLAUDE.md` has universal principles (style, TDD, plan-before-code). `~/jarvis/CLAUDE.md` has jarvis-specific workflows (ideas, STATUS, journal, shared). Claude Code natively loads both when in jarvis. | Jarvis rules don't bleed into non-jarvis sessions. Universal principles apply everywhere. |
| Non-software ideas | **Same structure, all markdown** — every active idea gets a workbench with CLAUDE.md, even if it's just docs. 99% will involve code anyway | Consistent pattern; don't overthink categories |
| Inter-project data | **Shared data layer** — projects can produce/consume data through a convention (`~/jarvis/shared/` or API patterns) | Projects aren't fully isolated; one project's output can feed another |
| Idea creation | **Conversational** — Harry talks, Claude creates the file. Two modes: quick dump (2-3 questions) or full brainstorm. Harry picks in the moment. | Never write markdown by hand; the whole point of Claude is to do the structuring |
| Workflow codification | **Soul now, skills later** — soul describes behaviors in plain language. Extract into formal skills only when a pattern becomes repetitive enough to warrant it | No premature infrastructure; skills are overhead until proven necessary |
| Idea capture location | **Anywhere** — any Claude session can create idea files in `~/jarvis/ideas/` regardless of current directory. Plus iMessage/phone for mobile capture. | Zero friction; never lose an idea because you're in the wrong terminal or away from computer |
| Cross-project intelligence | **Active dot-connecting** — Claude doesn't just organize, it proactively sees connections between projects/ideas and proposes them. STATUS.md + group field + Claude initiative. | Claude should be a thinking partner, not just a filing system |
| Engineering memory | **Journal directory** — starts with `patterns.md` + `antipatterns.md`, splits by domain (e.g., `scraping.md`, `api-design.md`) when a file hits ~100 lines. Claude reads only relevant files. | Accumulate knowledge across all projects; avoid repeating mistakes; scales without becoming unreadable |
| Shared data scope | **Workbench-only** — `shared/` is for workbench-to-workbench communication. Graduated projects in `~/projects/` must be self-contained for data ingestion (configurable paths, env vars, or own data dirs). | Prevents cross-directory coupling; graduated projects aren't fragile if jarvis moves |
| Soul size budget | **~100 lines max per file** — when `~/.claude/CLAUDE.md` or `~/jarvis/CLAUDE.md` grows past ~100 lines, extract detailed rules into reference files that Claude loads on-demand | Prevents soul from eating excessive context in every session |
| STATUS.md regeneration | **On-demand, not automatic** — Claude suggests regenerating during triage, or Harry asks. Auto-regen only when Claude is already modifying idea files (marginal cost). Not on every session start. | Avoids 15-30s latency penalty at session start as the system scales |
| Idea template growth | **Progressive sections** — seed template is minimal. As status progresses to exploring/ready, Claude adds architecture, tech stack, MVP scope sections through conversation. | Template matches idea maturity; don't front-load structure on half-formed thoughts |
| Context management | **Soul-driven** — soul tells Claude when/how to compress context, but ALWAYS ask before clearing | Optimize token usage without losing information Harry hasn't approved losing |
| Cerebro inspiration | **Cherry-pick ideas, not the system** — soul as constitution (values + personality), permission to disagree, anti-filler rules. Don't copy: Mac Mini infra, 47 agents, 8 MCP servers. Reference for Phase 2 only. | Cerebro validated the "soul = identity + principles" approach. Take the design philosophy, build our own implementation. |
| Soul identity structure | **Two halves per soul file** — identity (who Harry is, values, personality) + operating principles (how Claude should work). Cerebro's "four values" pattern is worth adopting. | Soul should feel like a person's constitution, not a system prompt. Identity half rarely changes; principles half evolves with usage. |
| Daily memory notes | **`memory/` directory** — daily notes (`YYYY-MM-DD.md`) written at session end, read at session start (today + yesterday). Lives alongside STATUS.md. | Three libraries: `ideas/` (what to build), `journal/` (what we've learned), `memory/` (what's happening now). Daily memory bridges sessions without bloating the soul. |
| Three-library structure | **ideas/ + journal/ + memory/** — three distinct libraries with different purposes and lifecycles | Ideas = future (what to build). Journal = permanent (what we've learned). Memory = ephemeral (what's happening now, rolling window). |
| Model routing | **Explicit table in soul** — Haiku for status/formatting/simple, Sonnet for most coding/debugging/research, Opus for architecture/complex/cross-project. Default: try cheaper first. | Prevents defaulting to Opus for everything. Makes cost-consciousness a first-class rule, not a suggestion. |
| Session initialization | **Read daily memory on jarvis session start** — on startup in jarvis, read today's + yesterday's memory notes to restore context | Cheap way to maintain continuity without loading full conversation history |
| Journal grouping logic | **Check-before-create + hierarchical splitting** — before creating a new domain file, check if an existing group covers it. Split groups further when they get long. | Prevents proliferation of tiny files. Groups stay coherent through explicit hierarchy. |
| Project graduation | **TODO** — come back to this. Process for moving projects from workbench/ to ~/projects/ is undefined. | Need more thought on what actually happens during graduation |
| Project CLAUDE.md template | **TODO** — draft exists but needs deeper brainstorming on what it should include and look like | Don't finalize yet; come back for dedicated design session |

---

## Overview

Build a personal "Jarvis" system around Claude Code that manages 10-25+ ideas across every category (software, research, experiments, content, business, automations, learning). The system must support three interaction modes:

1. **Autopilot** — Claude works autonomously, reports back with status
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
- Complex agent routing — oh-my-claudecode handles this in Phase 3
- The sheer scale — start with 2-3 terminals, not 47 agents

**TODO:** Harry needs to deeply think through his personal values and identity for the soul's identity half. This isn't something Claude can write for you — it needs to come from genuine self-reflection. The Cerebro "four values" pattern is a good template: pick 3-5 values that genuinely describe how you think and work.

---

## Phase 1: The Workshop

### Directory Structure

```
~/.claude/
├── CLAUDE.md                            ← THE SOUL (universal half)
│                                           Identity + operating principles. Loads in EVERY Claude session.
│                                           ~50-100 lines max.

~/jarvis/                                ← The hub (its own git repo)
├── .git/                                ← Tracks ideas, docs, shared — NOT workbenches
├── .gitignore                           ← Ignores workbench/*/
├── CLAUDE.md                            ← THE SOUL (jarvis half)
│                                           Jarvis-specific workflows. Only loads inside ~/jarvis/.
│                                           ~50-100 lines max.
├── IDEAS.md                             ← Quick-capture scratchpad (dump ideas fast)
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

**Serious projects live outside jarvis:**
```
~/projects/
├── finance-dashboard/                   ← Graduated project (own git repo)
│   ├── CLAUDE.md
│   └── ...
├── outreach-platform/                   ← Graduated project (own git repo)
│   ├── CLAUDE.md
│   └── ...
└── ...
```

The idea file (`~/jarvis/ideas/finance-dashboard.md`) has `workbench: ~/projects/finance-dashboard` to link them.

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

When an idea graduates to "active," Claude scaffolds a project directory. Small experiments and early-stage projects go under `~/jarvis/workbench/`. When a project gets serious (multiple components, its own deploy, collaborators), it moves to `~/projects/` or wherever makes sense, and the idea file's `workbench:` field updates to point there.

Each workbench is an independent project with its own git repo, its own CLAUDE.md, and its own structure appropriate to what it is.

### The Shared Data Layer (`shared/`)

Projects need to talk to each other. The `shared/` directory is the convention for this:

**How it works:**
- A project that **produces** data writes to a named folder in `~/jarvis/shared/` (e.g., `shared/reddit-data/`)
- A project that **consumes** data reads from that folder
- Each shared folder has a predictable structure: `latest.json` (or `latest.csv`, etc.) for the most recent data, and optionally dated archives

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

**Scope: workbench projects only.** `shared/` is for communication between projects inside `~/jarvis/workbench/`. When a project graduates to `~/projects/`, it must become self-contained — it should own its data ingestion (configurable paths via env vars, its own data directory, or an API). This prevents graduated projects from having a hard dependency on `~/jarvis/` existing at a specific path. Part of the graduation process is decoupling from `shared/`.

**Why not APIs?** In Phase 1, file-based sharing is simpler — no servers to run, no ports to manage. In Phase 2, projects that need real-time communication can graduate to actual APIs, but the `shared/` convention covers 90% of workbench cases.

### Idea Capture From Anywhere

Ideas don't only happen at your desk. The system supports capture from three surfaces:

**1. Any Claude terminal (already designed):**
You're working in `~/projects/finance-dashboard/` and have an idea. Say "new idea" — Claude creates the file in `~/jarvis/ideas/` without you switching terminals. The soul tells every Claude session where ideas live.

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

### Cross-Project Intelligence

Claude isn't just a filing system — it's a thinking partner that actively connects dots across the entire jarvis ecosystem.

**STATUS.md — The Big Picture:**
Auto-generated file at `~/jarvis/memory/STATUS.md` that gives Claude (and Harry) a snapshot of everything:

```markdown
# Jarvis Status
Generated: 2026-02-15 09:00

## Active Projects
- reddit-scraper (workbench/) — last touched 2h ago, 3 open TODOs
- outreach-platform (~/projects/) — last touched 1d ago, tests passing
- finance-dashboard (~/projects/) — last touched 3d ago, blocked on API key

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

## Connections Claude Sees
- reddit-scraper is scraping r/hiring for outreach. Could the same
  scraping pattern benefit learn-rust-by-building? (r/rust job posts
  could inform what to learn)
- finance-dashboard needs external data — could reddit-scraper be
  extended to pull r/personalfinance insights?
```

**Proactive Connection-Making:**
The soul instructs Claude to actively look for connections:
- When working on project A, consider whether the approach/data/pattern could benefit project B
- When brainstorming a new idea, check if existing projects already solve part of it
- When reviewing STATUS.md during triage, propose connections Harry might not have seen
- Always frame suggestions as proposals — "I noticed X, would it make sense to Y?" — never just do it

**The `group:` field in idea files:**
Ideas that are conceptually related share a group tag. Groups emerge organically — when Claude notices two ideas are related, it proposes grouping them. Groups appear in STATUS.md and help Claude reason about the ecosystem as a whole, not just individual projects.

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
- At the end of a jarvis session, Claude writes/updates today's note. Not a full transcript — a concise summary of what happened, what was decided, what's still open.
- Claude proposes the content; Harry approves before it's saved.

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

### Model Routing
- **Haiku**: status checks, formatting, simple file generation, quick questions
- **Sonnet**: most coding, debugging, research, standard implementation (DEFAULT)
- **Opus**: architecture, complex multi-file reasoning, cross-project analysis, design sessions
- Try cheaper first. Only escalate when the task genuinely needs deeper reasoning.

### Context Management
- When context is getting long and important info is captured in files, suggest compacting
- ALWAYS ask before clearing or compacting — never auto-clear
- The file IS the memory — once something is written to disk, the conversation is disposable
- When approaching rate limits, say so and suggest switching models or pausing

### Communication
- Be direct. No fluff, no filler, no performative agreement.
- When presenting options, lead with your recommendation
- Don't over-explain things I already know
- Flag risks early, don't bury them
- Don't pad responses to seem thorough. Short and right beats long and padded.

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

## The System
- Active projects live in ~/jarvis/workbench/ (small) or ~/projects/ (serious)
- Workbench-to-workbench data flows through ~/jarvis/shared/ (one writer per folder)
- Every project has a corresponding idea file — always keep them in sync

## Session Initialization
- On jarvis session start: read memory/today.md + memory/yesterday.md (if they exist)
- This restores context from previous sessions without loading full conversation history
- If neither exists, that's fine — start fresh

## Idea Workflow
- Harry says "new idea"? Ask: quick dump or brainstorm? Then create the idea file
- Harry wants to revisit an idea? Read the existing file, pick up where we left off
- Never make Harry write markdown by hand — that's Claude's job
- Ideas grow progressively: seed (minimal) → exploring (add research) → ready (add architecture, tech stack, MVP scope)

## Session Workflow
- Triage session? Offer to regenerate memory/STATUS.md, check IDEAS.md for anything to triage
- Working on a workbench? Read its CLAUDE.md first
- Finishing something? Update the idea file's status and last_touched, push if there's a remote
- Ending a session? Write/update memory/YYYY-MM-DD.md — summarize what happened, decisions made, open threads. Propose content, wait for approval.
- Scaffolding a new workbench? Create git repo, offer to create a GitHub remote (default yes)
- Never let a workbench exist without a remote for more than a week — remind Harry

## Cross-Project Thinking
- Before deep work, offer to check memory/STATUS.md to understand the bigger picture
- Actively look for connections between projects — shared patterns, reusable data, common pain points
- When working on project A, consider if the approach/data/pattern could benefit project B
- Always propose connections as suggestions — never just do it

## Engineering Journal
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
- Open individual project directories in Cursor when doing hands-on work
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

Terminal 3: cd ~/projects/project-b && claude
  → Deep work on Project B (serious project, lives outside jarvis)
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

`memory/STATUS.md` is a dashboard, not a live feed. It's regenerated on-demand, not automatically:
- **During triage**: When you start a triage session in `~/jarvis/`, Claude offers to regenerate STATUS.md. You can say yes or skip it.
- **When already touching idea files**: If Claude is creating/updating an idea file anyway, it regenerates STATUS.md as a side effect (marginal cost since it's already reading the files).
- **On explicit request**: "Update status" or "what's the state of everything" triggers regeneration.
- **NOT on every session start**: With 20+ idea files and 10 workbenches, auto-regen adds 15-30s latency. Not worth it for sessions where you're just doing focused work.
- **Cost at scale**: Reading frontmatter from all idea files + checking git status on workbenches. Fast with 5 projects, slow with 20. On-demand keeps this from becoming a tax on every session.

---

## Phase 2: The Studio (Growth Path)

> Don't build this yet. This section documents what gets added WHEN Phase 1 feels limiting.

### What Gets Added

```
~/jarvis/
├── ... (everything from Phase 1) ...
│
├── registry.json                    ← Machine-readable index of all ideas/projects
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

### Registry (`registry.json`)

Machine-readable companion to the ideas library. Auto-generated by Claude from the idea files. Enables:
- Filtering by status, category, priority
- Tracking last-touched dates
- Knowing which ideas have workbenches
- Data dependency graph (which projects produce/consume what)
- Feeding the daily digest

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
- [ ] **Project graduation process** — How does a project move from `workbench/` to `~/projects/`? What happens to git history? How does it decouple from `shared/`? How do data paths become configurable? Needs dedicated design session.
- [ ] **Rate limit guardrails** — Need real usage data before finalizing. Current thinking: 20+ tool call pause rule, daily `/usage` tracking, proactive model downgrade suggestions. Formalize after 1-2 weeks of real multi-terminal usage.
- [ ] **Soul refinement** — Current drafts are starting points. Refine both files after 1 week of real usage based on observed patterns.

### Resolved

- [x] **Soul symlink flaw** — Fixed: split into `~/.claude/CLAUDE.md` (universal, ~100 lines max) + `~/jarvis/CLAUDE.md` (jarvis-specific, ~100 lines max). Jarvis rules only load in jarvis context. Caught by Cursor review 2.
- [x] **Journal scalability** — Fixed: start with flat files, split by domain when a file hits ~100 lines. Directory structure supports this from day one. Claude reads only relevant files.
- [x] **STATUS.md regeneration** — Fixed: on-demand, not automatic. Claude offers during triage, auto-regens only when already modifying idea files. Avoids latency tax.
- [x] **Shared data coupling** — Fixed: `shared/` is workbench-only. Graduated projects must be self-contained. Decoupling from shared/ is part of graduation.
- [x] **Soul size budget** — Fixed: ~100 lines max per file. Extract details into reference files when growing.
- [x] **Idea template growth** — Fixed: progressive sections that grow with status (seed → exploring → ready → active).
- [x] **iMessage integration** — Moved to Phase 2. Terminal + IDEAS.md covers 90% of capture needs.
- [x] **Budget tracking** — Resolved: model selection strategy in soul + daily `/usage` checks + Phase 2 cost logging.
- [x] **Backups** — Resolved: default to creating GitHub remotes on scaffold, soul reminds to push, Phase 2 weekly check routine.
- [x] **Shared data write safety** — Resolved: "one writer per shared folder" as hard rule, `_meta.json` for every shared dataset.

---

## Next Steps

### Phase 1 Build Order

1. **Harry's values** — Self-reflect on 3-5 personal values for the soul identity half (not a Claude task)
2. **Brain dump** — Get all 10-25 ideas out of Harry's head into idea files (conversational)
3. **Design project CLAUDE.md template** — Brainstorm session (flagged TODO, high priority)
4. **Scaffold `~/jarvis/`** — Create the directory structure: ideas/, journal/, memory/, workbench/, shared/, docs/ref/, .gitignore, templates, both CLAUDE.md soul files
5. **Write the soul** — Finalize both `~/.claude/CLAUDE.md` and `~/jarvis/CLAUDE.md` with real values, model routing table, session init rules
6. **Promote 1-2 ideas** — Graduate the most exciting ones to workbenches
7. **Work in the system** — Use it for a week, note what's missing. Daily memory notes will start accumulating.
8. **Iterate** — Adjust structure, soul, and conventions based on real usage
9. **Resolve graduation process** — Once a project actually needs to move, design the process from real experience

### Phase 2 (When Triggered)

10. **Orchestration** — Add oh-my-claudecode, routines, morning script
11. **Heartbeat system** — Local LLM scheduler for routine checks and agent wake-ups
12. **iMessage integration** — Design and build mobile capture
13. **Rate limit guardrails** — Formalize daily budgets, pause rules, model routing enforcement
14. **Cost logging** — Track spend over time in ~/jarvis/logs/
15. **Pipeline formalization** — pipelines.json for complex multi-project data flows
