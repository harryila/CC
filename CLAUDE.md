# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is an **analysis repository** containing 25 cloned Claude Code ecosystem repos plus three authored documents. It is not a software project — there is no build system, no tests, and no application code to run. The cloned repos are read-only reference material; all original work lives in the root-level markdown files.

## Key Files (Root Level)

- `MASTER_ANALYSIS.md` — Safety audit and detailed analysis of all 25 repos (the primary deliverable)
- `MY_CLAUDE_CODE_PLAN.md` — Personal strategy for building an autonomous AI dev system using these repos
- `README.md` — One-paragraph overview

## The 25 Cloned Repos

Each subdirectory is an independent Git repo cloned for local analysis. They span several categories:

**Methodology/Workflows:** `superpowers/`, `BMAD-METHOD/`, `compound-engineering-plugin/`, `everything-claude-code/`
**Planning/Memory:** `planning-with-files/`, `claude-mem/`, `beads/`
**MCP Servers:** `context7/`, `chrome-devtools-mcp/`, `repomix/`
**Orchestration/Agents:** `oh-my-claudecode/`, `claude-flow/`, `ralph/`, `agents/` (wshobson), `awesome-claude-code-subagents/`, `awesome-claude-code-toolkit/`
**Project Management:** `ccpm/`
**Browser Automation:** `agent-browser/`
**Templates/Reference:** `awesome-claude-code/`, `claude-code-templates/`, `claude-code-infrastructure-showcase/`
**Utilities:** `claude-code-router/`, `qmd/`
**Domain Skills:** `ui-ux-pro-max-skill/`, `marketingskills/`

## Architecture: The Recommended Stack

The analysis concludes with an optimal 8-12 repo setup organized in layers:

```
Layer 1 — Methodology:    superpowers (plan → implement → test → review)
Layer 2 — Planning:       planning-with-files (task_plan.md, findings.md, progress.md)
Layer 3 — Memory:         claude-mem (SQLite + ChromaDB vector search)
Layer 4 — Task Tracking:  beads (git-backed JSONL in .beads/)
Layer 5 — Orchestration:  oh-my-claudecode (multi-agent, smart model routing)
Layer 6 — Tools (MCP):    context7, chrome-devtools-mcp, repomix
Layer 7 — Agents:         wshobson/agents (cherry-pick per need)
Layer 8 — Autonomous:     ralph (PRD-driven execution loops)
```

## Key Overlap Groups (Install One Per Group)

These repos compete — installing multiples from the same group causes conflicts:

| Group | Competing Repos | Recommended Pick |
|-------|----------------|-----------------|
| Methodology | superpowers, BMAD-METHOD, compound-engineering | superpowers |
| Orchestration | oh-my-claudecode, claude-flow | oh-my-claudecode |
| Agent Collections | wshobson/agents, awesome-subagents, awesome-toolkit | wshobson/agents |
| Browser | chrome-devtools-mcp, agent-browser | chrome-devtools-mcp |

## Working With This Repo

- **Read-only analysis**: The cloned repos should not be modified. Edits belong in the root markdown files.
- **Cross-repo searches**: When investigating how a concept (e.g., hooks, MCP servers, planning patterns) is implemented across repos, search across all subdirectories.
- **Language mix**: Most repos are TypeScript/Node.js (~60%), with one Go repo (`beads/`), some Python, and many that are pure markdown/config.
- **No build/test commands**: This repo has no package.json, Makefile, or test suite. Individual cloned repos have their own build systems but are not meant to be built from this parent directory.
