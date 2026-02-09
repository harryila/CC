# Claude Code Repos - Master Analysis

> 25 repositories analyzed for building the ultimate Claude Code infrastructure.  
> All repos cloned, explored, safety-checked, and cross-referenced.  
> Analysis date: February 7, 2026

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Safety Report](#safety-report)
3. [Individual Repo Analyses](#individual-repo-analyses)
4. [Overlap & Conflict Analysis](#overlap--conflict-analysis)
5. [Recommended Stack](#recommended-stack)
6. [Phased Installation Plan](#phased-installation-plan)
7. [What NOT to Install Together](#what-not-to-install-together)

---

## Executive Summary

### The Bottom Line

Of the 25 repos analyzed, **all are safe** (no malicious code found in any). However, they vary significantly in quality, utility, and overlap. Many provide similar functionality, so installing all 25 would create conflicts and bloat.

**The optimal setup uses 8-12 repos**, selected to avoid overlap while maximizing capability:

| Category | Winner | Why |
|----------|--------|-----|
| Methodology/Workflows | **superpowers** | Best skill-based workflow system, marketplace install, actively maintained |
| Persistent Planning | **planning-with-files** | Lightweight, hook-driven, complements any methodology |
| Production Configs | **everything-claude-code** | Battle-tested agents/hooks/rules, cherry-pick what you need |
| Documentation MCP | **context7** | Up-to-date library docs, simple install, high-value |
| Browser Debugging MCP | **chrome-devtools-mcp** | Official Google project, comprehensive DevTools access |
| Persistent Memory | **claude-mem** | Cross-session memory with vector search, auto-setup |
| Task Tracking | **beads** | Git-backed, merge-safe, AI-optimized |
| Codebase Packing | **repomix** | Pack repos for AI consumption, security scanning |
| Multi-Agent Orchestration | **oh-my-claudecode** | Zero-config, magic keywords, HUD, intelligent routing |
| UI/UX Skills | **ui-ux-pro-max-skill** | Searchable design databases, multi-stack support |
| Marketing Skills | **marketingskills** | 25 modular marketing skills, pure markdown |
| Autonomous Loops | **ralph** | Simple, focused, PRD-driven autonomous execution |

---

## Safety Report

### All 25 Repos: SAFE

| # | Repo | Safety | Notes |
|---|------|--------|-------|
| 1 | qmd | All Clear | Fully local, no cloud dependencies |
| 2 | oh-my-claudecode | All Clear | Legitimate tool spawning only |
| 3 | BMAD-METHOD | All Clear | npm/git only, MIT license |
| 4 | ralph | All Clear | Simple bash script, no network calls |
| 5 | claude-code-infrastructure-showcase | All Clear | TypeScript hooks, read-only patterns |
| 6 | awesome-claude-code | All Clear | Reference list only, no executable code |
| 7 | ccpm | All Clear | GitHub CLI (`gh`) calls only |
| 8 | claude-code-router | All Clear | Localhost proxy, API keys in user config |
| 9 | awesome-claude-code-subagents | All Clear | Markdown files only |
| 10 | awesome-claude-code-toolkit | All Clear | Markdown/JSON configs only |
| 11 | claude-flow | All Clear | Built-in security module, input validation |
| 12 | agents (wshobson) | All Clear | Plugin configs only, MIT license |
| 13 | beads | All Clear | Git operations only, Go codebase |
| 14 | claude-mem | All Clear | Localhost only, CORS restricted, AGPL-3.0 |
| 15 | planning-with-files | All Clear | Local file operations only |
| 16 | ui-ux-pro-max-skill | All Clear | Local CSV search only |
| 17 | marketingskills | All Clear | Pure markdown, no executable code |
| 18 | superpowers | All Clear | Git fetch with 3s timeout for updates only |
| 19 | context7 | All Clear | Connects to context7.com (Upstash-owned) |
| 20 | everything-claude-code | All Clear | Standard tools (prettier, tsc, git) |
| 21 | claude-code-templates | All Clear | Standard npm package |
| 22 | repomix | All Clear | Secretlint prevents credential leaks |
| 23 | agent-browser | All Clear | Playwright patterns, Apache-2.0 |
| 24 | chrome-devtools-mcp | All Clear | Official Google project, opt-out telemetry |
| 25 | compound-engineering-plugin | All Clear | Standard bash/Python scripts |

### Minor Telemetry Notes (opt-out available)
- **chrome-devtools-mcp**: Sends usage stats to Google's Clearcut (`play.googleapis.com/log`). Opt-out: set `CHROME_DEVTOOLS_MCP_DISABLE_TELEMETRY=true`
- **context7**: API key optional; IP encryption for privacy (AES-256-CBC)
- **claude-mem**: AGPL-3.0 license (requires source disclosure for network deployments)

---

## Individual Repo Analyses

### 1. superpowers (obra/superpowers)
**Category**: Methodology & Workflow Framework  
**Stars**: ~46.8k | **Quality**: HIGH | **Install**: Medium (marketplace for Claude Code)

**What it is**: A skills library that adds structured workflows (TDD, debugging, planning, code review) via composable skills that agents invoke automatically.

**How it works**: Plugin installs via marketplace. A `SessionStart` hook injects the `using-superpowers` skill, which enforces checking for relevant skills before any action. 14 skills in `skills/*/SKILL.md` with YAML frontmatter.

**Key Skills**:
- `using-superpowers` - Core discipline (mandatory skill checking)
- `brainstorming` - Design refinement before implementation
- `writing-plans` / `executing-plans` - Structured planning with checkpoints
- `subagent-driven-development` - Autonomous execution with two-stage review
- `test-driven-development` - RED-GREEN-REFACTOR enforcement
- `systematic-debugging` - 4-phase root cause analysis
- `dispatching-parallel-agents` - Concurrent subagent workflows
- `verification-before-completion` - Gate function pattern

**Install**: `/plugin install superpowers`

---

### 2. everything-claude-code (affaan-m/everything-claude-code)
**Category**: Production Config Collection  
**Stars**: ~41.7k | **Quality**: HIGH | **Install**: Medium

**What it is**: Battle-tested collection of 14 agents, 30+ skills, 30+ commands, 8 rule categories, and hooks from an Anthropic hackathon winner.

**Key Highlights**:
- Language-specific rules (TypeScript, Python, Go, Java)
- Continuous learning system (auto-extracts patterns into skills)
- Memory persistence hooks (save/load context across sessions)
- Pre-configured MCP servers (GitHub, Supabase, Vercel, Railway, ClickHouse)
- TDD enforcement with 80%+ coverage requirements

**Best used for**: Cherry-picking specific agents, rules, and hooks you need rather than installing everything.

**Install**: `/plugin install everything-claude-code@everything-claude-code` + manual rule copy

---

### 3. BMAD-METHOD (bmad-code-org/BMAD-METHOD)
**Category**: Agile AI Development Framework  
**Stars**: ~34.6k | **Quality**: HIGH | **Install**: Simple (`npx bmad-method install`)

**What it is**: Full agile framework with 9+ specialized agent personas (PM, Architect, Developer, QA, etc.), 50+ workflows, and scale-adaptive planning.

**Key Highlights**:
- Named personas: Analyst (Mary), PM (John), Architect (Winston), Developer (Amelia), QA (Quinn)
- Phase-based workflows: Analysis -> Planning -> Solutioning -> Implementation
- Party Mode: Multiple agents in one session
- Module ecosystem for extensibility
- IDE-agnostic (Claude Code, Cursor, Windsurf)

**Best for**: Teams wanting a full agile methodology with AI. Heavier than superpowers.

---

### 4. compound-engineering-plugin (EveryInc/compound-engineering-plugin)
**Category**: Workflow Plugin + Marketplace  
**Stars**: ~7.4k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: Plugin with the Plan -> Work -> Review -> Compound cycle. 29 agents, 25 commands, 16 skills, 2 MCP servers.

**Key Commands**: `/workflows:plan`, `/workflows:work`, `/workflows:review`, `/workflows:compound`

**Unique Value**: Knowledge compounding - captures and reuses successful patterns across projects.

---

### 5. planning-with-files (OthmanAdi/planning-with-files)
**Category**: Persistent Planning Skill  
**Stars**: ~13.3k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: "Manus-style" 3-file planning pattern (`task_plan.md`, `findings.md`, `progress.md`) with hooks for automatic plan re-reading.

**Key Mechanism**: PreToolUse hook reads plan before decisions; PostToolUse reminds updates; Stop verifies completion. Files survive context resets.

**Why it's special**: Lightweight, focused, works alongside any methodology. Session recovery after `/clear`.

---

### 6. context7 (upstash/context7)
**Category**: MCP Server - Documentation  
**Stars**: ~45k | **Quality**: HIGH | **Install**: Simple (one command)

**What it is**: MCP server providing up-to-date library documentation and code examples. Reduces hallucinated API usage.

**Install**: `claude mcp add context7 -- npx -y @upstash/context7-mcp`

---

### 7. chrome-devtools-mcp (ChromeDevTools/chrome-devtools-mcp)
**Category**: MCP Server - Browser Debugging  
**Stars**: ~23.5k | **Quality**: HIGH | **Install**: Simple (one command)

**What it is**: Official Google MCP server exposing Chrome DevTools (26+ tools: input automation, navigation, performance, network, debugging).

**Install**: `claude mcp add chrome-devtools --scope user npx chrome-devtools-mcp@latest`

---

### 8. agent-browser (vercel-labs/agent-browser)
**Category**: Browser Automation CLI  
**Stars**: ~13.1k | **Quality**: HIGH | **Install**: Simple

**What it is**: Headless browser automation optimized for AI agents. Rust CLI + Node.js daemon, snapshot-ref workflow, cloud provider support.

**vs chrome-devtools-mcp**: agent-browser is CLI-first with cloud providers; chrome-devtools-mcp is MCP-native with DevTools profiling. They're complementary but most users only need one.

---

### 9. repomix (yamadashy/repomix)
**Category**: Codebase Packing Tool  
**Stars**: ~21.7k | **Quality**: HIGH | **Install**: Simple

**What it is**: Packs entire codebases into a single AI-friendly file. Tree-sitter compression (~70% token reduction), security scanning, MCP server.

**Install**: `npx repomix@latest` or MCP: `claude mcp add repomix -- npx -y repomix --mcp`

---

### 10. beads (steveyegge/beads)
**Category**: Task/Issue Tracker  
**Stars**: ~15.3k | **Quality**: HIGH | **Install**: Simple

**What it is**: Git-backed graph issue tracker. JSONL storage in `.beads/`, hash-based IDs prevent merge conflicts, dependency tracking, auto-sync daemon.

**Key Feature**: `bd ready` lists tasks with no blockers. Multi-agent safe.

**Install**: `npm install -g @beads/bd` then `bd init`

---

### 11. claude-mem (thedotmack/claude-mem)
**Category**: Persistent Memory System  
**Stars**: ~24.6k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: Cross-session memory using SQLite + ChromaDB vector search. Hooks capture observations, generate summaries, inject relevant memory into future sessions.

**Key Features**: 5 lifecycle hooks, MCP tools for memory search, web viewer UI at localhost:37777, privacy controls via `<private>` tags.

**Install**: `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem`

---

### 12. qmd (tobi/qmd)
**Category**: Local Search Engine  
**Stars**: ~7.1k | **Quality**: HIGH | **Install**: Medium

**What it is**: On-device search engine for markdown files. BM25 + vector semantic search + LLM re-ranking, all local.

**Best for**: Users with large collections of notes, docs, meeting transcripts. Requires Bun and ~2GB for models.

---

### 13. oh-my-claudecode (Yeachan-Heo/oh-my-claudecode)
**Category**: Multi-Agent Orchestration  
**Stars**: ~5.2k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: Multi-agent orchestration with intelligent model routing, 32 agents, 32 skills, magic keywords (autopilot, ultrawork, ralph, eco), HUD statusline.

**Key Features**: Zero-config, natural language interface, automatic parallelization, persistent execution, rate limit auto-resume, 30-50% cost savings via smart routing.

---

### 14. claude-flow (ruvnet/claude-flow)
**Category**: Enterprise Orchestration  
**Stars**: ~13.8k | **Quality**: HIGH | **Install**: Medium

**What it is**: Enterprise multi-agent platform with 60+ agents, swarm coordination (hierarchical/mesh/ring/star), consensus protocols (Raft, Byzantine), self-learning, vector memory.

**vs oh-my-claudecode**: claude-flow is more complex/powerful (enterprise); oh-my-claudecode is simpler/zero-config. Most users should start with oh-my-claudecode.

---

### 15. ralph (snarktank/ralph)
**Category**: Autonomous Agent Loop  
**Stars**: ~9.7k | **Quality**: HIGH | **Install**: Simple

**What it is**: Bash script that loops Claude Code repeatedly until all PRD items are complete. Fresh context per iteration, persistent learnings via git.

**Key Mechanism**: Reads `prd.json`, picks highest-priority incomplete story, implements, commits if passing, marks complete. Exits when done.

---

### 16. ccpm (automazeio/ccpm)
**Category**: Project Management Workflow  
**Stars**: ~7.1k | **Quality**: HIGH | **Install**: Simple

**What it is**: Spec-driven PM using GitHub Issues. PRD -> Epic -> Task -> Issue -> Code -> Commit flow with worktrees for parallel execution.

**Best for**: Teams using GitHub Issues who want structured, traceable development.

---

### 17. claude-code-router (musistudio/claude-code-router)
**Category**: Model Router/Proxy  
**Stars**: ~27.4k | **Quality**: HIGH | **Install**: Medium

**What it is**: HTTP proxy that routes Claude Code requests to different LLM providers (OpenRouter, DeepSeek, Gemini, Ollama, etc.).

**Key Feature**: Use Claude Code's UI with any LLM provider. Cost optimization by routing simple tasks to cheaper models.

---

### 18. agents (wshobson/agents)
**Category**: Agent Marketplace  
**Stars**: ~28k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: 73 plugins with 112 agents, 146 skills, 79 tools. Granular plugin architecture - install only what you need.

**Key Feature**: Three-tier model strategy (Opus/Sonnet/Haiku). 100% agent coverage across 24 categories.

---

### 19. awesome-claude-code-subagents (VoltAgent/awesome-claude-code-subagents)
**Category**: Subagent Collection  
**Stars**: ~9.8k | **Quality**: MEDIUM-HIGH | **Install**: Simple

**What it is**: 127+ specialized subagents in 10 categories. Community-driven, with interactive installer.

**Overlap with wshobson/agents**: Significant. Both provide large agent collections. wshobson/agents is more structured; this is more community-driven.

---

### 20. awesome-claude-code-toolkit (rohitg00/awesome-claude-code-toolkit)
**Category**: Comprehensive Toolkit  
**Stars**: ~319 | **Quality**: HIGH | **Install**: Simple

**What it is**: 135 agents, 35 skills, 42 commands, 120 plugins, 19 hooks, 15 rules, 7 templates, 6 MCP configs. SkillKit marketplace integration.

**Overlap**: Heavy overlap with wshobson/agents and awesome-claude-code-subagents. Choose one collection.

---

### 21. awesome-claude-code (hesreallyhim/awesome-claude-code)
**Category**: Curated Reference List  
**Stars**: ~23.1k | **Quality**: HIGH | **Install**: N/A

**What it is**: Curated list of Claude Code resources. Useful for discovery. No install needed - just bookmark it.

---

### 22. claude-code-templates (davila7/claude-code-templates)
**Category**: CLI Component Catalog  
**Stars**: ~19.7k | **Quality**: HIGH | **Install**: Simple (`npx`)

**What it is**: CLI tool and web catalog for browsing/installing Claude Code components. 5000+ files. Web UI at aitmpl.com.

**Use**: `npx claude-code-templates@latest` to browse and install individual components.

---

### 23. claude-code-infrastructure-showcase (diet103/claude-code-infrastructure-showcase)
**Category**: Reference Patterns  
**Stars**: ~8.8k | **Quality**: HIGH | **Install**: Medium

**What it is**: Production-tested hook patterns for auto-activating skills. Solves the "skills don't activate" problem.

**Key Pattern**: `skill-activation-prompt.ts` hook analyzes prompts and suggests relevant skills. Copy the patterns you need.

---

### 24. ui-ux-pro-max-skill (nextlevelbuilder/ui-ux-pro-max-skill)
**Category**: Domain Skill - UI/UX  
**Stars**: ~29.1k | **Quality**: HIGH | **Install**: Simple (`npm install -g uipro-cli`)

**What it is**: Searchable design databases (50+ styles, 97 color palettes, 57 font pairings, 99 UX guidelines). Python BM25 search engine.

---

### 25. marketingskills (coreyhaines31/marketingskills)
**Category**: Domain Skill - Marketing  
**Stars**: ~6.5k | **Quality**: HIGH | **Install**: Simple (marketplace)

**What it is**: 25 modular marketing skills (CRO, copywriting, SEO, paid ads, pricing, growth). Pure markdown, no dependencies.

---

## Overlap & Conflict Analysis

### Overlap Group 1: Methodology Frameworks (PICK ONE)

These all provide structured workflows. Installing multiple will create confusion and conflicting instructions.

| Repo | Approach | Complexity | Best For |
|------|----------|------------|----------|
| **superpowers** | Skills-based, TDD/debugging focus | Medium | Solo devs, quality-focused teams |
| **BMAD-METHOD** | Full agile with personas | High | Teams wanting complete agile methodology |
| **compound-engineering** | Plan-Work-Review-Compound | Low | Teams wanting simple workflow structure |
| **everything-claude-code** | Production configs collection | Medium | Cherry-picking specific patterns |

**Recommendation**: Start with **superpowers** (best balance of structure and flexibility). Add **compound-engineering** if you want the compounding pattern. Use **everything-claude-code** as a reference to cherry-pick specific agents/rules.

### Overlap Group 2: Agent Collections (PICK ONE)

These all provide large collections of pre-built agents. Heavy duplication between them.

| Repo | Agents | Skills | Plugins | Approach |
|------|--------|--------|---------|----------|
| **wshobson/agents** | 112 | 146 | 73 | Structured marketplace |
| **awesome-claude-code-subagents** | 127+ | — | 10 | Community collection |
| **awesome-claude-code-toolkit** | 135 | 35 | 120 | Comprehensive toolkit |

**Recommendation**: Pick **wshobson/agents** (most structured, granular plugin architecture). Use others as reference to find agents not in wshobson's collection.

### Overlap Group 3: Orchestration Systems (PICK ONE)

These manage multi-agent coordination. Running multiple orchestrators will conflict.

| Repo | Complexity | Best For | Key Feature |
|------|------------|----------|-------------|
| **oh-my-claudecode** | Low (zero-config) | Most users | Magic keywords, HUD, auto-routing |
| **claude-flow** | High (enterprise) | Large teams | Swarm topologies, consensus protocols |
| **ralph** | Minimal | Autonomous loops | PRD-driven iteration |
| **ccpm** | Medium | GitHub teams | GitHub Issues integration |

**Recommendation**: **oh-my-claudecode** for most users. **ralph** can complement it for autonomous loop tasks. Avoid **claude-flow** unless you have enterprise needs.

### Overlap Group 4: Browser Automation (PICK ONE or BOTH)

| Repo | Approach | Best For |
|------|----------|----------|
| **chrome-devtools-mcp** | MCP-native, DevTools integration | Debugging, performance profiling |
| **agent-browser** | CLI-first, cloud providers | Testing, CI/CD, cloud execution |

**Recommendation**: **chrome-devtools-mcp** for web development debugging. Add **agent-browser** only if you need cloud browser providers or CI/CD integration.

### Overlap Group 5: Planning Systems (PICK ONE)

| Repo | Approach | Mechanism |
|------|----------|-----------|
| **planning-with-files** | 3-file pattern with hooks | Auto re-reads plans before every action |
| **superpowers** | Skills for planning | Manual invocation of planning skills |
| **BMAD-METHOD** | Phase-based workflows | Structured PRD/architecture documents |
| **compound-engineering** | /workflows:plan command | Creates structured project plans |

**Recommendation**: **planning-with-files** is the most lightweight and can work alongside any methodology. Its hook-based approach (auto re-reads plan before every tool use) is unique.

### Overlap Group 6: Memory/Context Persistence

| Repo | Approach | Mechanism |
|------|----------|-----------|
| **claude-mem** | Full memory system | SQLite + ChromaDB + hooks + MCP |
| **planning-with-files** | File-based context | 3 markdown files survive resets |
| **everything-claude-code** | Session state hooks | Save/load session context |

**Recommendation**: **claude-mem** for comprehensive memory. **planning-with-files** for lightweight task-level persistence. They complement each other.

### Overlap Group 7: Reference Lists (NO INSTALL - BOOKMARK)

These are informational resources, not tools:
- **awesome-claude-code** - Master curated list
- **claude-code-templates** - CLI for browsing components
- **claude-code-infrastructure-showcase** - Reference hook patterns

---

## Recommended Stack

### The Optimal Claude Code Setup (12 repos)

```
CORE METHODOLOGY (pick one)
  superpowers ..................... Structured workflows, TDD, debugging, planning

PERSISTENT PLANNING
  planning-with-files ............ Hook-driven 3-file planning pattern

MCP SERVERS (install all three)
  context7 ....................... Up-to-date library documentation
  chrome-devtools-mcp ............ Browser debugging and performance
  repomix (MCP mode) ............. Codebase packing and analysis

MEMORY & TRACKING
  claude-mem ..................... Cross-session persistent memory
  beads ......................... Git-backed task/issue tracking

ORCHESTRATION
  oh-my-claudecode .............. Multi-agent coordination, smart routing

AGENT COLLECTION (pick from)
  wshobson/agents ............... 73 plugins, install only what you need

DOMAIN SKILLS (as needed)
  ui-ux-pro-max-skill ........... If doing UI/UX work
  marketingskills ............... If doing marketing work

AUTONOMOUS EXECUTION
  ralph ......................... PRD-driven autonomous loops
```

### What to Skip (and Why)

| Repo | Why Skip | Use Instead |
|------|----------|-------------|
| BMAD-METHOD | Too heavy for most; superpowers is lighter | superpowers |
| claude-flow | Enterprise complexity; overkill for most | oh-my-claudecode |
| ccpm | Niche (GitHub Issues PM); beads is more flexible | beads |
| awesome-claude-code-subagents | Overlaps with wshobson/agents | wshobson/agents |
| awesome-claude-code-toolkit | Overlaps with wshobson/agents | wshobson/agents |
| agent-browser | chrome-devtools-mcp covers most needs | chrome-devtools-mcp |
| claude-code-router | Only needed for non-Anthropic models | Skip unless needed |
| awesome-claude-code | Reference only, no install needed | Bookmark it |
| claude-code-templates | Reference/CLI, no permanent install | Use npx when needed |
| infrastructure-showcase | Reference patterns, copy what you need | Copy specific hooks |

### What to Keep as References

These repos are valuable for browsing/cherry-picking but shouldn't be "installed" as systems:

1. **everything-claude-code** - Cherry-pick specific agents, hooks, and rules
2. **claude-code-infrastructure-showcase** - Copy the skill-activation hook pattern
3. **compound-engineering-plugin** - Borrow the compound workflow if superpowers doesn't cover it
4. **awesome-claude-code** - Bookmark for discovering new tools
5. **claude-code-templates** - Use `npx claude-code-templates@latest` to browse components

---

## Phased Installation Plan

### Phase 1: Foundation (Day 1)
*Essential tools that every Claude Code user should have.*

```bash
# 1. Context7 MCP - Up-to-date library docs
claude mcp add context7 -- npx -y @upstash/context7-mcp

# 2. Chrome DevTools MCP - Browser debugging
claude mcp add chrome-devtools --scope user npx chrome-devtools-mcp@latest

# 3. Repomix MCP - Codebase packing
claude mcp add repomix -- npx -y repomix --mcp

# 4. Superpowers - Core methodology
# (via Claude Code marketplace)
/plugin install superpowers

# 5. Planning with Files - Persistent planning
/plugin marketplace add OthmanAdi/planning-with-files
/plugin install planning-with-files@planning-with-files
```

**What you get**: Structured workflows, persistent planning, up-to-date docs, browser debugging, and codebase analysis.

### Phase 2: Memory & Tracking (Day 2-3)
*Persistent context across sessions.*

```bash
# 6. Claude-Mem - Cross-session memory
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem

# 7. Beads - Task/issue tracking
npm install -g @beads/bd
cd your-project && bd init
```

**What you get**: Memory that persists across sessions, structured task tracking with dependency awareness.

### Phase 3: Orchestration & Agents (Day 4-5)
*Multi-agent coordination and specialized agents.*

```bash
# 8. Oh-My-ClaudeCode - Multi-agent orchestration
/plugin marketplace add Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode

# 9. Agent marketplace - Install specific agents you need
/plugin marketplace add wshobson/agents
# Then install specific plugins, e.g.:
/plugin install python-pro
/plugin install security-auditor
/plugin install architect-reviewer
```

**What you get**: Multi-agent coordination with smart model routing, specialized agents for your tech stack.

### Phase 4: Domain Skills & Advanced (Week 2)
*Add as needed based on your work.*

```bash
# 10. UI/UX Skill (if doing frontend work)
npm install -g uipro-cli && uipro init

# 11. Marketing Skills (if doing marketing)
/plugin marketplace add coreyhaines31/marketingskills
/plugin install marketingskills

# 12. Ralph (for autonomous task loops)
/plugin marketplace add snarktank/ralph
/plugin install ralph

# 13. QMD (if you have lots of markdown docs/notes)
bun install -g github:tobi/qmd
# Configure collections and run: qmd embed
```

### Phase 5: Advanced/Optional (As Needed)

```bash
# Claude Code Router (only if you want non-Anthropic models)
npm install -g @musistudio/claude-code-router

# Agent Browser (only if you need cloud browser automation)
npm install -g agent-browser && agent-browser install
```

---

## What NOT to Install Together

### Conflict Risk: HIGH

| Combo | Problem |
|-------|---------|
| superpowers + BMAD-METHOD | Both inject methodology at session start; conflicting workflow instructions |
| oh-my-claudecode + claude-flow | Both try to orchestrate agents; will fight for control |
| Multiple agent collections (all 3) | Duplicate agents create confusion; Claude Code may invoke wrong one |

### Conflict Risk: MEDIUM

| Combo | Problem |
|-------|---------|
| claude-mem + everything-claude-code hooks | Both define SessionStart/SessionEnd hooks; may conflict |
| superpowers + compound-engineering | Both provide planning/review workflows; instructions may conflict |
| Multiple planning systems | planning-with-files hooks + other planning → duplicate plan management |

### Conflict Risk: LOW (Generally Safe Together)

| Combo | Notes |
|-------|-------|
| Any MCP servers together | MCP servers are independent; all safe together |
| beads + any methodology | beads is task tracking, not methodology |
| Domain skills + anything | ui-ux and marketing skills are isolated |
| ralph + any orchestrator | ralph is a standalone loop, not an orchestrator |
| repomix + any tool | repomix is a standalone packing tool |

---

## Quick Reference: All 25 Repos at a Glance

| # | Repo | Type | Quality | Install | Recommended? |
|---|------|------|---------|---------|-------------|
| 1 | qmd | MCP/Search | HIGH | Medium | If you have markdown docs |
| 2 | oh-my-claudecode | Orchestrator | HIGH | Simple | YES - core pick |
| 3 | BMAD-METHOD | Methodology | HIGH | Simple | Alternative to superpowers |
| 4 | ralph | Autonomous Loop | HIGH | Simple | YES - for auto tasks |
| 5 | infrastructure-showcase | Reference | HIGH | Medium | Copy patterns only |
| 6 | awesome-claude-code | Curated List | HIGH | N/A | Bookmark only |
| 7 | ccpm | Project Mgmt | HIGH | Simple | If using GitHub Issues |
| 8 | claude-code-router | Model Router | HIGH | Medium | If using non-Anthropic models |
| 9 | awesome-subagents | Agent Collection | MED-HIGH | Simple | Use wshobson/agents instead |
| 10 | awesome-toolkit | Toolkit | HIGH | Simple | Use wshobson/agents instead |
| 11 | claude-flow | Enterprise Orch | HIGH | Medium | Only for enterprise needs |
| 12 | agents (wshobson) | Agent Marketplace | HIGH | Simple | YES - cherry-pick agents |
| 13 | beads | Task Tracker | HIGH | Simple | YES - core pick |
| 14 | claude-mem | Memory System | HIGH | Simple | YES - core pick |
| 15 | planning-with-files | Planning Skill | HIGH | Simple | YES - core pick |
| 16 | ui-ux-pro-max-skill | UI/UX Skill | HIGH | Simple | If doing UI/UX work |
| 17 | marketingskills | Marketing Skill | HIGH | Simple | If doing marketing |
| 18 | superpowers | Methodology | HIGH | Medium | YES - core pick |
| 19 | context7 | MCP/Docs | HIGH | Simple | YES - core pick |
| 20 | everything-claude-code | Config Collection | HIGH | Medium | Cherry-pick from it |
| 21 | claude-code-templates | CLI Catalog | HIGH | Simple | Use npx when needed |
| 22 | repomix | Codebase Packer | HIGH | Simple | YES - core pick |
| 23 | agent-browser | Browser CLI | HIGH | Simple | If need cloud browsers |
| 24 | chrome-devtools-mcp | MCP/Browser | HIGH | Simple | YES - core pick |
| 25 | compound-engineering | Workflow Plugin | HIGH | Simple | Alternative to superpowers |

---

## Final Notes

### Impressive Quality Across the Board
Every single repo scored HIGH quality (with one MEDIUM-HIGH). The Claude Code ecosystem is remarkably mature. There are no "junk" repos in this list.

### The Ecosystem is Young and Overlapping
Many repos solve the same problems differently. This is natural for a young ecosystem. Over time, winners will emerge and consolidation will happen. For now, being selective is key.

### Start Small, Add Incrementally
The biggest mistake would be installing everything at once. Start with Phase 1 (5 tools), use them for a week, then add Phase 2. This lets you understand each tool before adding complexity.

### Keep an Eye On
- **superpowers** and **everything-claude-code** are the most actively maintained and likely to improve
- **oh-my-claudecode** is rapidly evolving with new execution modes
- **context7** and **chrome-devtools-mcp** are backed by companies (Upstash and Google) so they'll be maintained long-term
- **claude-mem** is solving a real pain point (cross-session memory) that Anthropic may eventually build natively

### The "If Anthropic Builds It" Risk
Some of these tools fill gaps that Anthropic may eventually address natively:
- Persistent memory (claude-mem) - Anthropic is likely working on this
- Task tracking (beads) - Could become native
- Documentation lookup (context7) - Could become native
- Model routing (claude-code-router) - Anthropic may add multi-model support

Even so, these tools provide value *today* and their implementations will inform whatever Anthropic builds natively.
