# Jarvis Infrastructure Design

> Personal AI development system — from idea library to autonomous execution.
> Status: DESIGNING
> Started: February 10, 2026

---

## Design Decisions Log

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Project location | **Hybrid** — small experiments in `~/jarvis/workbench/`, serious projects get their own location (e.g., `~/projects/`) | Keeps jarvis from becoming a junk drawer, but small stuff doesn't need top-level dirs |
| Git strategy | **Jarvis root is a repo, workbenches are gitignored** — each workbench/project has its own git repo | Ideas/docs are versioned together; project code doesn't pollute jarvis history |
| Project linking | **Path in idea file** — the idea file's `workbench:` field stores the path to the project wherever it lives | Single source of truth, no symlinks to maintain |
| Soul scope | **Global** — `~/jarvis/soul.md` symlinked to `~/.claude/CLAUDE.md`, applies to every Claude session | Consistency everywhere; Claude always feels like "your" Claude |
| Non-software ideas | **Same structure, all markdown** — every active idea gets a workbench with CLAUDE.md, even if it's just docs. 99% will involve code anyway | Consistent pattern; don't overthink categories |
| Inter-project data | **Shared data layer** — projects can produce/consume data through a convention (`~/jarvis/shared/` or API patterns) | Projects aren't fully isolated; one project's output can feed another |
| Idea creation | **Conversational** — Harry talks, Claude creates the file. Two modes: quick dump (2-3 questions) or full brainstorm. Harry picks in the moment. | Never write markdown by hand; the whole point of Claude is to do the structuring |
| Workflow codification | **Soul now, skills later** — soul describes behaviors in plain language. Extract into formal skills only when a pattern becomes repetitive enough to warrant it | No premature infrastructure; skills are overhead until proven necessary |
| Idea capture location | **Anywhere** — any Claude session can create idea files in `~/jarvis/ideas/` regardless of current directory. Plus iMessage/phone for mobile capture. | Zero friction; never lose an idea because you're in the wrong terminal or away from computer |
| Cross-project intelligence | **Active dot-connecting** — Claude doesn't just organize, it proactively sees connections between projects/ideas and proposes them. STATUS.md + group field + Claude initiative. | Claude should be a thinking partner, not just a filing system |
| Engineering memory | **Journal** — `~/jarvis/journal/patterns.md` (what worked) and `antipatterns.md` (what failed and why). Referenced before decisions, updated after learnings. | Accumulate knowledge across all projects; avoid repeating mistakes; reuse winning patterns |
| Context management | **Soul-driven** — soul tells Claude when/how to compress context, but ALWAYS ask before clearing | Optimize token usage without losing information Harry hasn't approved losing |
| Project graduation | **TODO** — come back to this. Process for moving projects from workbench/ to ~/projects/ is undefined. | Need more thought on what actually happens during graduation |
| Project CLAUDE.md template | **TODO** — draft exists but needs deeper brainstorming on what it should include and look like | Don't finalize yet; come back for dedicated design session |

---

## Overview

Build a personal "Jarvis" system around Claude Code that manages 10-25+ ideas across every category (software, research, experiments, content, business, automations, learning). The system must support three interaction modes:

1. **Autopilot** — Claude works autonomously, reports back with status
2. **Directed** — Harry gives direction, Claude executes, Harry reviews
3. **Hands-on** — Harry and Claude work together in real-time, with Cursor as the surgical cockpit for manual editing

### Approach

**Phase 1 (The Workshop):** Ideas library + isolated workbenches + soul CLAUDE.md + Cursor integration + shared data conventions. Get productive immediately.

**Phase 2 (The Studio):** Add orchestration, scheduling, background routines, and multi-project coordination on top of the Workshop foundation.

### Principles

- **Files you can read** — No databases, no hidden state. Everything is markdown, JSON, or code you can open and understand.
- **Claude builds it, Harry directs it** — Infrastructure is set up by Claude, maintained by Claude, but transparent enough that Harry can tweak anything.
- **Start sparse, grow dense** — An idea starts as a one-paragraph file. It only gets more structure when it needs it.
- **No premature infrastructure** — Don't build Phase 2 tooling until Phase 1 is full and creaking.
- **Cursor is the cockpit** — When Harry needs to go hands-on (edit files, review diffs, inspect visually), Cursor is the interface. Claude Code is the engine underneath.
- **Projects can talk** — Projects aren't fully siloed. When one project produces data another needs, there's a clean convention for sharing it.

---

## Phase 1: The Workshop

### Directory Structure

```
~/jarvis/                                ← The hub (its own git repo)
├── .git/                                ← Tracks ideas, docs, soul, shared — NOT workbenches
├── .gitignore                           ← Ignores workbench/*/
├── soul.md                              ← The "soul" — symlinked to ~/.claude/CLAUDE.md
├── IDEAS.md                             ← Quick-capture scratchpad (dump ideas fast)
├── STATUS.md                            ← Auto-generated dashboard (all projects, data flows, groups)
│
├── ideas/                               ← The library — one file per idea
│   ├── _template.md                     ← Template for new idea files
│   ├── leetcode-for-ai.md
│   ├── reddit-scraper-digest.md
│   ├── personal-finance-dashboard.md
│   ├── learn-rust-by-building.md
│   └── ...
│
├── journal/                             ← Engineering memory — persists across all projects
│   ├── patterns.md                      ← What worked: reusable code, good decisions, winning approaches
│   └── antipatterns.md                  ← What failed: approaches that broke, libraries that caused issues, traps
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
└── docs/
    └── plans/                           ← Design docs like this one
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
- The producing project's CLAUDE.md documents what it writes and the schema
- The consuming project's CLAUDE.md documents what it reads and from where
- The idea file for each project lists its `produces:` and `consumes:` dependencies
- `shared/README.md` is the master reference of all data flows

**Why not APIs?** In Phase 1, file-based sharing is simpler — no servers to run, no ports to manage. In Phase 2, projects that need real-time communication can graduate to actual APIs, but the `shared/` convention covers 90% of cases.

### Idea Capture From Anywhere

Ideas don't only happen at your desk. The system supports capture from three surfaces:

**1. Any Claude terminal (already designed):**
You're working in `~/projects/finance-dashboard/` and have an idea. Say "new idea" — Claude creates the file in `~/jarvis/ideas/` without you switching terminals. The soul tells every Claude session where ideas live.

**2. Quick text file (already designed):**
Open `~/jarvis/IDEAS.md` in any editor, dump a one-liner. Triage later.

**3. iMessage / Phone (Phase 1.5):**
Text or call Jarvis from your phone. Use cases:
- Walking and have an idea → text Jarvis a one-liner → it gets added to IDEAS.md or becomes a seed idea file
- Waiting somewhere → text "what's the status of the reddit scraper?" → get a summary back
- On a call and hear something relevant → text "look into X for the outreach project" → gets captured

**Implementation approach (design later, but the pieces):**
- Apple Shortcuts automation that watches for messages to a specific contact ("Jarvis")
- Shortcut triggers a script on your Mac (via SSH, or local if Mac is awake)
- Script runs Claude in headless mode (`claude --print` or similar) with the message as input
- Claude reads the jarvis system, processes the request, writes the result
- Response sent back via iMessage through Shortcuts

This is a Phase 1.5 feature — design and build it after the core Workshop is working but before Phase 2 orchestration. It's high-value because ideas happen everywhere.

### Cross-Project Intelligence

Claude isn't just a filing system — it's a thinking partner that actively connects dots across the entire jarvis ecosystem.

**STATUS.md — The Big Picture:**
Auto-generated file at `~/jarvis/STATUS.md` that gives Claude (and Harry) a snapshot of everything:

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

### The Soul (`soul.md`)

Lives at `~/jarvis/soul.md` and gets symlinked to `~/.claude/CLAUDE.md`. Loaded into every Claude Code session everywhere. Contains operating principles, workflow preferences, and tells Claude how to behave across all projects.

**Draft contents:**

```markdown
# Soul

## Who I Am
- Builder, experimenter, learning fast
- Ideas-first thinker — quantity of exploration matters
- Prefers seeing things work over perfect architecture

## How Claude Operates
- Always plan before coding (use superpowers)
- Always verify before claiming done
- When unsure, ask — don't guess
- Keep things simple until complexity is earned
- Use TDD for anything that ships; skip for experiments
- Before technical decisions, check ~/jarvis/journal/ for relevant patterns or warnings
- After learning something (good or bad), add it to the journal

## Workflow Rules
- New session in ~/jarvis/? Check IDEAS.md for anything to triage
- Working on a workbench? Read its CLAUDE.md first
- Finishing something? Update the idea file's status and last_touched
- Creating a new project? Use the workbench scaffold pattern
- Harry says "new idea"? Ask: quick dump or brainstorm? Then create the idea file — works from ANY directory
- Harry wants to revisit an idea? Read the existing file, pick up where we left off
- Never make Harry write markdown by hand — that's Claude's job
- Regenerate ~/jarvis/STATUS.md whenever idea files or project state changes

## Context Management
- When context is getting long and the important info is captured in files, suggest compacting
- ALWAYS ask Harry before clearing or compacting — never auto-clear
- After creating or updating an idea file, the conversation about it CAN be cleared — the file IS the memory
- Use Haiku/Sonnet for lightweight tasks (status checks, triage), Opus for deep work

## Cross-Project Thinking
- Glance at ~/jarvis/STATUS.md before deep work to understand the bigger picture
- Actively look for connections between projects — shared patterns, reusable data, common pain points
- When working on project A, consider if the approach/data/pattern could benefit project B
- When brainstorming a new idea, check if existing projects already solve part of it
- Always propose connections as suggestions — "I noticed X, would it make sense to Y?" — never just do it

## Soul Evolution
- If you notice a pattern in how Harry works that isn't captured here, propose adding it
- Don't edit the soul silently — suggest the change and explain why
- The soul is a living document that grows through conversation

## Communication Style
- Be direct, skip fluff
- When presenting options, lead with your recommendation
- Don't over-explain things I already know
- Flag risks early, don't bury them

## The Jarvis System
- Ideas live in ~/jarvis/ideas/
- Active projects live in ~/jarvis/workbench/ (small) or ~/projects/ (serious)
- Inter-project data flows through ~/jarvis/shared/
- Engineering knowledge lives in ~/jarvis/journal/ (patterns.md + antipatterns.md)
- Every project has a corresponding idea file — always keep them in sync
- STATUS.md is the big picture — all projects, groups, data flows, connections
```

(Draft — refine after a week of real usage.)

---

## Idea File Template

```markdown
---
title: [Idea Name]
status: seed
category: [software | experiment | automation | content | business | learning]
created: YYYY-MM-DD
last_touched: YYYY-MM-DD
priority: [high | medium | low | someday]
workbench: null
produces: []
consumes: []
---

# [Idea Name]

## What
[One paragraph: what is this idea?]

## Why
[Why does this matter to you? What problem does it solve?]

## Success Looks Like
[How would you know this worked?]

## Notes
[Anything else — links, inspirations, constraints, open questions]
```

**Field notes:**
- `workbench:` starts as `null`, becomes `workbench/project-name` or `~/projects/project-name` when activated
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

- [ ] **Project CLAUDE.md template** — Draft exists conceptually but needs dedicated brainstorming on what it should include, look like, and how it connects back to the jarvis system. Do NOT finalize until this session happens.
- [ ] **Project graduation process** — How does a project move from `workbench/` to `~/projects/`? What happens to git history, shared data paths, idea file references? Needs more thought — no solution chosen yet.
- [ ] **iMessage integration design** — The concept is defined (Apple Shortcuts → script → Claude headless → respond via iMessage) but the actual implementation needs a dedicated design session.
- [ ] **Soul refinement** — Current draft is a starting point. Refine after 1 week of real usage based on observed patterns.
- [ ] **STATUS.md generation** — How/when does it get regenerated? On every session start? On demand? As a hook? Needs decision.

---

## Next Steps

1. **Brain dump** — Get all 10-25 ideas out of Harry's head into idea files
2. **Refine the soul** — Draft soul.md with real preferences
3. **Scaffold `~/jarvis/`** — Create the directory structure, .gitignore, templates, journal
4. **Promote 1-2 ideas** — Graduate the most exciting ones to workbenches
5. **Work in the system** — Use it for a week, note what's missing
6. **Iterate** — Adjust structure based on real usage
7. **iMessage integration** (Phase 1.5) — Design and build mobile capture
8. **Open TODOs** — Come back to project CLAUDE.md template and graduation process
