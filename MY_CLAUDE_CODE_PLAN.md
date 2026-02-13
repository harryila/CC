# My Claude Code System — Vision, Strategy & Plan

> Personal blueprint for building an autonomous AI development system.  
> Started: February 2026

---

## The Vision

Build a **Jarvis-like autonomous development system** where I can:

- **Write up what I want** in plain language (like a PRD or brief)
- **Have it be autonomously executed** — agents plan, implement, test, and deliver
- **Spin up specialized agents** — expert architect, security reviewer, frontend dev, etc.
- **Check in with me** at key decision points (architecture, deploys, risky changes)
- **Scale horizontally** — run multiple projects in parallel simultaneously
- **Persistent memory** — the system remembers past decisions, patterns, and context across sessions

### Example Scenarios

**Scenario 1**: "Build a new LeetCode-style platform but for AI usage"
- System creates PRD, breaks into epics/stories
- Scaffolds repo, implements auth, problems DB, evaluation harness
- Frontend dev agent builds UI while backend agent builds API
- Security reviewer checks everything before deploy
- Checks in with me at architecture decisions and before going live

**Scenario 2**: "Scrape and understand the /r/claude subreddit"
- Spins up a scraper service (Reddit API or HTML fallback)
- Stores raw data → cleans → embeds → builds retrieval
- Creates a weekly digest and searchable interface
- Runs in parallel while other projects are active

**Scenario 3**: "Refactor the auth module across three microservices"
- Analyzes all three repos, creates a plan
- Implements changes with TDD (tests first, then code)
- Code review agent checks each PR
- Merges only after all tests pass and I approve

---

## The Architecture

### Mental Model

```
Me (human)
  │
  ├── Cursor ← My cockpit. Where I review, inspect, hands-on code.
  │
  └── Claude Code ← The engine. Where agents plan, execute, and deliver.
        │
        ├── Methodology Layer (superpowers)
        │     └── Enforces: plan → implement → test → review
        │
        ├── Planning Layer (planning-with-files)
        │     └── Persistent task_plan.md, findings.md, progress.md
        │
        ├── Memory Layer (claude-mem)
        │     └── Cross-session memory with vector search
        │
        ├── Task Tracking (beads)
        │     └── Git-backed issues with dependency graphs
        │
        ├── Orchestration (oh-my-claudecode)
        │     └── Multi-agent coordination, smart model routing
        │
        ├── Expert Agents (wshobson/agents)
        │     └── 112 specialists: security, frontend, backend, etc.
        │
        ├── Autonomous Loops (ralph)
        │     └── PRD-driven loops that run until all tasks complete
        │
        └── Tools (MCP servers)
              ├── context7 — Live library documentation
              ├── chrome-devtools — Browser debugging
              └── repomix — Codebase packing & analysis
```

### The Layers Explained

**Layer 1 — Orchestrator (the "manager")**
Decides what projects are active, what tasks run in parallel, what needs my approval, which expert agent gets which task, and what context each agent receives.

**Layer 2 — Workers (the "agents")**
Each worker is one of:
- Claude Code instances (best for code changes, refactors, implementation)
- Thin agents for research/scrape/summarize (don't need repo write access)
- Specialist agents (security reviewer, data engineer, frontend expert, etc.)

**Layer 3 — Tools (the "hands")**
Scraping services, databases, deployment pipelines, browser automation, etc. Claude Code calls tools I control — it doesn't "wing it" on the internet.

### The Workflow Pattern

```
Intent → Plan → Decompose → Dispatch → Execute → Verify → Checkpoint → Compound
```

1. **Intent**: I write what I want (plain language or PRD)
2. **Plan**: Planner agent creates structured plan with milestones
3. **Decompose**: Plan breaks into epics → tasks → issues
4. **Dispatch**: Tasks assigned to appropriate specialist agents
5. **Execute**: Agents implement with TDD, tests, and commits
6. **Verify**: Review agents check security, performance, spec compliance
7. **Checkpoint**: System asks me to approve at high-risk points
8. **Compound**: Successful patterns extracted and saved for future use

---

## Platform Decisions

### Why Claude Code (not just Cursor)

| Criterion | Cursor | Claude Code |
|-----------|--------|-------------|
| Autonomy | Human-in-the-loop always | Can run autonomously |
| Reasoning depth | Good for inline edits | Best-in-class for architecture |
| Agent-native | No agent system | Hooks, skills, subagents, MCP |
| Orchestration | None | Multi-agent coordination |
| Ceiling | Hits limits on complex tasks | Scales with infrastructure |
| Ecosystem | Extensions only | 25+ repos building agent infra |

**Decision**: Claude Code = engine, Cursor = cockpit. Not competitors — different layers.

### Why not the others?

- **Codex (OpenAI)**: Strong at structured execution, but weaker at architectural reasoning. Watch it, don't build on it yet.
- **OpenCode (OSS)**: Good for inspectability, but thin ecosystem. Several of our repos support it as a secondary platform.
- **Claude Flow**: Enterprise-grade but overkill. Oh-my-claudecode is simpler and sufficient to start.

---

## The Stack

### What's Installed

**MCP Servers (global, all projects):**

| Server | Purpose | Status |
|--------|---------|--------|
| context7 | Up-to-date library documentation | Installed |
| chrome-devtools | Browser debugging & performance | Installed |
| repomix | Codebase packing for AI analysis | Installed |

**Plugins (global, all projects):**

| Plugin | Purpose | Status |
|--------|---------|--------|
| superpowers | Structured workflows (TDD, debugging, planning, review) | Phase 1 |
| planning-with-files | Persistent 3-file planning with hooks | Phase 1 |
| claude-mem | Cross-session persistent memory | Phase 2 |
| oh-my-claudecode | Multi-agent orchestration | Phase 3 |
| wshobson/agents | 112 specialist agents (install per-need) | Phase 3 |
| ralph | Autonomous PRD-driven loops | Phase 4 |
| marketingskills | 25 marketing skills (CRO, SEO, copywriting) | Phase 4 (as needed) |
| ui-ux-pro-max-skill | Searchable design databases | Phase 4 (as needed) |

### Phase 1: Foundation (install first)
- superpowers — enforces plan-before-code, TDD, systematic debugging
- planning-with-files — creates task_plan.md, findings.md, progress.md; hooks auto-re-read before every action
- context7 MCP — live library docs
- chrome-devtools MCP — browser debugging
- repomix MCP — codebase analysis

### Phase 2: Memory & Tracking (after a few days)
- claude-mem — remembers decisions, patterns, context across sessions
- beads — git-backed task tracking with dependency graphs (`npm install -g @beads/bd`)

### Phase 3: Orchestration & Agents (after a week)
- oh-my-claudecode — multi-agent coordination, magic keywords (autopilot, ultrawork), HUD, smart model routing
- wshobson/agents — cherry-pick specific specialist agents needed

### Phase 4: Advanced (as needed)
- ralph — autonomous execution loops for PRD-driven tasks
- ui-ux-pro-max-skill — if doing UI/UX work
- marketingskills — if doing marketing work
- qmd — if I have large collections of markdown docs/notes to search

---

## How to Use It

### Daily Workflow

```bash
# 1. Navigate to project
cd ~/Desktop/my-project

# 2. Launch Claude Code
claude

# 3. Give it a task (superpowers kicks in automatically)
"Add user authentication with JWT tokens and refresh flow"

# Claude Code will:
#   - Check for relevant skills (superpowers)
#   - Create task_plan.md, findings.md, progress.md (planning-with-files)
#   - Pull current JWT library docs (context7)
#   - Plan before coding
#   - Implement with TDD
#   - Verify before completing
```

### Autonomous Mode (Phase 4)

```bash
# Write a PRD
# Let ralph loop until everything is done
# Each iteration: fresh context, picks next task, implements, tests, commits
```

### Multi-Agent Mode (Phase 3)

```
# Inside Claude Code, use magic keywords:
"autopilot: build the entire user dashboard with tests"
"ultrawork: refactor the database layer across all services"
```

---

## Reference: The 25 Repos Analyzed

Full analysis in [MASTER_ANALYSIS.md](./MASTER_ANALYSIS.md).

### Core Picks (using these)

| Repo | What it does | Role in my system |
|------|-------------|-------------------|
| superpowers | Skills-based workflow framework | Methodology layer |
| planning-with-files | Persistent 3-file planning | Planning layer |
| claude-mem | Cross-session memory | Memory layer |
| beads | Git-backed task tracker | Task tracking |
| oh-my-claudecode | Multi-agent orchestration | Orchestration layer |
| wshobson/agents | 112 specialist agents | Expert agents |
| ralph | Autonomous PRD loop | Autonomous execution |
| context7 | Live library docs MCP | Tool |
| chrome-devtools-mcp | Browser debugging MCP | Tool |
| repomix | Codebase packer MCP | Tool |

### Reference Repos (cherry-pick from)

| Repo | What to take from it |
|------|---------------------|
| everything-claude-code | Specific agents, hooks, and rules as needed |
| infrastructure-showcase | The skill-activation hook pattern |
| compound-engineering | The compound workflow (Plan → Work → Review → Compound) |
| awesome-claude-code | Bookmark for discovering new tools |
| claude-code-templates | Use `npx claude-code-templates@latest` to browse components |

### Skipped (and why)

| Repo | Why |
|------|-----|
| BMAD-METHOD | Too heavy; superpowers is lighter and sufficient |
| claude-flow | Enterprise overkill; oh-my-claudecode is simpler |
| ccpm | Niche GitHub Issues PM; beads is more flexible |
| awesome-claude-code-subagents | Overlaps with wshobson/agents |
| awesome-claude-code-toolkit | Overlaps with wshobson/agents |
| agent-browser | chrome-devtools-mcp covers most needs |
| claude-code-router | Only needed for non-Anthropic models |

---

## The Gap: What I Still Need to Build

The 25 repos cover ~80% of the Jarvis vision. The remaining gap is the **multi-project orchestration layer** — the thing that manages multiple projects running simultaneously.

### What exists:
- oh-my-claudecode handles multi-agent within ONE project
- ralph handles autonomous loops for ONE project
- beads tracks tasks for ONE project

### What's missing:
- A dispatcher that says "work on Project A AND Project B AND scrape Reddit, all at once"
- A project registry (which repos, which PRDs, which agents)
- Budget management (token limits, time limits, cost caps)
- Cross-project approval gates (Slack webhook or terminal prompt)
- Observability dashboard (what's running, what's blocked, what needs me)

### Plan to fill the gap:
Build a thin orchestration layer (~500 lines) that:
1. Maintains a project queue (JSON or Redis)
2. Launches Claude Code instances per project
3. Routes specialist agents to the right project
4. Surfaces checkpoints for my approval
5. Tracks spend and enforces budgets

Claude Code itself can build this.

---

## Key Principles

1. **Start small, add incrementally** — Phase 1 first, then layer on complexity
2. **Understand each tool before stacking** — use superpowers for a week before adding orchestration
3. **Don't install competing tools** — one methodology, one orchestrator, one agent collection
4. **Claude Code is the engine, Cursor is the cockpit** — different layers, not competitors
5. **Build the Jarvis AROUND Claude Code** — it won't become Jarvis by itself; I add orchestration, permissions, experts, and checkpoints
6. **Autonomy requires guardrails** — budgets, sandboxing, rollback, evaluation, permissions
7. **The winners will be agent operating systems** — understanding agent orchestration now = massive edge in 18-24 months
