# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains custom skills for Claude Code. Skills extend Claude Code with specialized agents, commands, and always-on rules that are installed via symlinks into `~/.claude/`.

## Installation Commands

```bash
# Install all skills (unified installer)
bash install.sh

# Install specific skill only
bash install.sh agent-manager

# Show installation status
bash install.sh --status

# Update to latest
bash install.sh --update

# Copy project-level agent templates to current project
bash install.sh --init-project

# Uninstall all skills
bash install.sh --uninstall
```

Individual skill installers (`agent-manager/install.sh`, `context-plane/install.sh`) still work standalone.

## Architecture

### AgentManager Skill (v2.0.1)

Automatic task dispatch to specialized sub-agents. Every user message is classified by intent and routed to the best agent — UI work goes to the UI specialist, git operations go to github-sync, code audits go to the bug finder, and so on. No manual intervention needed.

**Components:**

| File | Installed To | Purpose |
|------|--------------|---------|
| `agent-managing.md` | `~/.claude/rules/` | Always-on rule that routes tasks to appropriate agents |
| `agent-manager.md` | `~/.claude/commands/` | Manual `/agent-manager` command for force-dispatch |
| `agents/user/*.md` | `~/.claude/agents/` | User-level agent definitions (symlinked) |
| `agents/project-templates/*.md` | `.claude/agents/` | Project-level agents (copied on init) |

**Agent Types:**

- **User-level** (`agents/user/`): Shared across all projects, installed via symlink
  - `feature-planner.md` — Feature scoping and planning
  - `github-sync.md` — Git/GitHub operations
  - `local-dev-runner.md` — Build, test, environment ops
  - `codex-cli.md` — Codex CLI integration (code review, research)
  - `mcp-tool-developer.md` — MCP tool server development

- **Project-level** (`agents/project-templates/`): Copied per-project, can be customized
  - `bug-finder-refiner.md` — Code quality audits (memory: project)
  - `ui-specialist.md` — UI/UX work (memory: project)
  - `db-specialist.md` — Database, schema, migrations (memory: project)
  - `test-specialist.md` — Testing, pytest, fixtures (memory: project)
  - `rag-specialist.md` — RAG pipelines, embeddings (memory: project)
  - `api-specialist.md` — API endpoints, auth, RBAC (memory: project)

**Routing Priority:**

1. Tier 1 (immediate): `ui-specialist`, `bug-finder-refiner`, `github-sync`, `db-specialist`, `test-specialist`
2. Tier 2 (domain match): `local-dev-runner`, `feature-planner`, `mcp-tool-developer`, `rag-specialist`, `api-specialist`, `codex-agent`, `Explore`, `Plan`
3. Main thread: trivial fixes, clarification questions

**State Tracking:**

Modifications are tracked in `~/.claude/projects/<project-key>/context-plane-state.json` under the `agent_manager_tracking` key. After 10+ modifications, suggests running `/context-plane review`.

### ContextPlane Skill (v1.0.0)

Autonomous memory lifecycle management and session persistence for Claude Code. Keeps CLAUDE.md, MEMORY.md, and session context clean, organized, and within size limits — automatically.

**Components:**

| File | Installed To | Purpose |
|------|--------------|---------|
| `session-resume.md` | `~/.claude/rules/` | Always-on rule for autonomous working memory |
| `context-plane.md` | `~/.claude/commands/` | Dispatcher for all `/context-plane` subcommands |
| `checkpoint.md` | `~/.claude/commands/` | Capture and persist session context |
| `review-memory.md` | `~/.claude/commands/` | Autonomous review with proposal + approval |
| `prune-memory.md` | `~/.claude/commands/` | Automated pruning with approval |
| `move-memory.md` | `~/.claude/commands/` | Move sections between memory files |
| `overflow.md` | `~/.claude/commands/` | Move detailed MEMORY.md sections to topic files |
| `pin.md` | `~/.claude/commands/` | Pin entries to protect from pruning |
| `restore.md` | `~/.claude/commands/` | List and restore pre-modification snapshots |

**Session Resume Rule:**

The `session-resume.md` rule runs on every user message. It reads `session-context.md` from the project auto memory directory and uses compact-and-write mode to keep the file under 75 lines. Cross-session staleness checks flag entries older than 4 hours.

## Adding New Skills

Follow the existing pattern:
1. Create a subdirectory for the skill
2. Create `skill.json` manifest declaring rules, commands, agents, hooks
3. Include `VERSION` file with semver string
4. Create `install.sh` for standalone installation (optional but recommended)
5. Add skill name to `skills.json` at repo root

See `agent-manager/skill.json` for manifest format reference.
