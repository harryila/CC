# Jarvis Soul Draft — `~/jarvis/CLAUDE.md`

> Planning draft. This will become the actual `~/jarvis/CLAUDE.md` file after iteration.
> Loads ONLY when Claude runs inside `~/jarvis/` or its subdirectories. ~50-100 lines max.

---

## Design Choices

| Choice | Rationale |
|--------|-----------|
| **No duplication with universal soul** | Universal soul handles: identity, communication style, decision-making, model routing, token efficiency, context management, escape hatches, boundaries. Jarvis soul handles ONLY jarvis-specific systems and workflows. |
| **Session init is tiered** | Three tiers of loading: always (soul files, auto), cheap default (today's memory note), and on-demand (status/registry). Prevents session start from becoming a token tax. |
| **Memory auto-write, no approval gate** | Previous design required Harry to approve daily memory notes. This created friction that led to skipping. An imperfect note that exists beats a perfect one that doesn't. Harry corrects next session if wrong. |
| **Three-layer intelligence stack** | Structural (registry.json) + Knowledge (journal/) + Creative (opportunities.md). Each type of cross-project intelligence lives where it naturally belongs. No duplication between layers. |
| **Kill/archive cadence is explicit** | Without entropy pressure, the idea library becomes a graveyard. Forced decisions (archive/pause/promote) during weekly triage keep it lean. |
| **Health checks are suggestions, not gates** | Claude flags issues during triage but Harry decides what to act on. System should inform, not block. |
| **Token efficiency repeated here** | Some rules (git diff, RTK) are in the universal soul. Jarvis adds jarvis-specific rules: registry.json is cheap to query, use it instead of scanning idea files. |
| **Registry as backbone** | registry.json is the structured data layer. STATUS.md is a view rendered from it. Idea files are the source of truth, registry is the index. Soul instruction: "when you modify an idea file, update registry.json to match." |

## What Needs Adjustment

- [ ] **Line count is the biggest risk** — Current draft is ~95 lines. That's at the upper limit. After real usage, some sections may need to be extracted to `docs/ref/` reference files.
- [ ] **Session init "auto" loading is technically inaccurate** — CLAUDE.md files auto-load, but reading memory notes requires Claude to actively Read the file on session start. The soul instructs this, but it's not truly automatic like CLAUDE.md loading. Should this be a hook instead? (Cursor review 5 said no — CLAUDE.md instruction is sufficient.)
- [ ] **Health checks list may grow** — Currently 7 checks. As real usage reveals more failure modes, this section will push past the line budget. Extract to `docs/ref/health-checks.md` when it happens.
- [ ] **Triage flow is implicit** — The soul describes what to do during triage across multiple sections (session workflow, health checks, kill/archive, cross-project thinking). There's no single "triage checklist" that sequences these. Could be confusing. Consider a `docs/ref/triage-checklist.md` that consolidates the full triage procedure.
- [ ] **Workbench vs projects distinction is TODO** — Soul references both but the difference is undefined. Will clarify through usage.
- [ ] **Model routing is duplicated** — Both universal and jarvis soul mention model routing. Universal handles the general principle (sub-agent delegation). Jarvis adds the specific application (delegate status compilation). Could be confusing. Consider keeping model routing ONLY in universal soul.
- [ ] **Missing: project CLAUDE.md template reference** — When Claude scaffolds a new workbench, what goes in its CLAUDE.md? The template is still a TODO in the main plan. Jarvis soul should reference it once designed.
- [ ] **Missing: what "triage" means concretely** — The word "triage" appears 8 times but is never defined. Is it a dedicated session type? A mode? A command? Should there be a trigger phrase?

---

## The Draft

```markdown
# Jarvis System

## Three Libraries
- ideas/ — what to build (one file per idea, created through conversation)
- journal/ — what we've learned (patterns + antipatterns, split by domain when long)
- memory/ — what's happening now (STATUS.md + daily notes)

## Structured Data
- registry.json — machine-readable index of all ideas/projects. Update when idea files change.
- opportunities.md — cross-project observations. Add during work, Harry processes during triage.

## The System
- Active projects live in ~/jarvis/workbench/ (experimental) or ~/jarvis/projects/ (serious)
- Inter-project data flows through ~/jarvis/shared/ (one writer per folder)
- Every project has a corresponding idea file AND registry entry — keep both in sync

## Session Initialization

**Memory load (default, cheap):**
- Read ~/jarvis/memory/YYYY-MM-DD.md (today)
- If today's file doesn't exist, read yesterday's (only one day back)
- Do NOT read older memory unless explicitly requested

**Status load (only when needed):**
- If Harry says "triage," "what's going on," "status" → read registry.json, render STATUS.md
- For deep status (explicit request): full scan of idea files + git status to rebuild registry
- Otherwise skip status entirely by default

## Idea Workflow
- Harry says "new idea"? Ask: quick dump or brainstorm? Then create the idea file.
- Revisiting an idea? Read the file, pick up where we left off.
- Never make Harry write markdown by hand — structuring is Claude's job.
- Ideas grow progressively: seed → exploring → ready → active.
- When creating/updating an idea file, update registry.json to match.

## Session Workflow
- **Triage?** Read registry.json, offer to regen STATUS.md, check IDEAS.md, present opportunities.md.
- **Working on a project?** Read its CLAUDE.md first.
- **Finishing something?** Update idea file status + last_touched, update registry, push if remote exists.
- **Ending a session?** Auto-write memory/YYYY-MM-DD.md. No approval gate. Say "Memory note updated."
- **Scaffolding?** Create git repo, offer GitHub remote (default yes). Never let a workbench go
  a week without a remote — remind Harry.

## Cross-Project Thinking
- During work: notice a creative connection? Add a checkbox entry to opportunities.md.
- During triage: present opportunities.md items for Harry to process (act, dismiss, defer).
- Structural connections (data flows, groups) → registry.json. Don't duplicate.
- Knowledge connections (patterns) → journal/. Don't duplicate.

## Health Checks (during triage)
- Zombie projects: active status but no commits in 2+ weeks? Suggest pausing.
- Unpushed work: local commits without a remote? Remind to push.
- Stale shared data: _meta.json last_updated > 7 days but consumer active? Flag it.
- IDEAS.md backlog > 10 items? Nudge toward triage.
- Auto-memory: spot-check /memory for active projects — flag stale entries.
- Soul drift: doing something not described in soul? Propose adding it.
- Journal relevance: entries referencing unused tools/libraries? Flag as stale.
- These are suggestions, not gates. Harry decides.

## Kill / Archive Cadence

**Weekly triage — flag:**
- Any idea in seed/exploring not touched in 30 days
- Any active workbench/project not touched in 14 days

**For each flagged item:**
- Archive (default): mark status: archived, archived_reason, last_touched. Not deletion.
- Pause: requires a next-review date.
- Promote: define next action + scope.

## Engineering Journal
- Only add when a lesson generalizes across future projects. Project-specific → memory/.
- Before technical decisions, check relevant journal files.
- Before creating a new domain file, check if an existing group covers it.
- Split by domain when a file hits ~100 lines.

## Boundaries
- Never delete idea files — archive them instead.
- Never write to shared/ folders this project doesn't own (check _meta.json).
- When modifying idea files, always update registry.json to match.
- This file stays under ~100 lines. Extract details to docs/ref/ when needed.
```
