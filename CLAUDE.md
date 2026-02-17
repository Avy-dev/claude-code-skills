# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains custom skills for Claude Code. Skills extend Claude Code with specialized agents, commands, and always-on rules that are installed via symlinks into `~/.claude/`.

## Installation Commands

```bash
# Install agent-router skill (rule, command, and user-level agents)
bash agent-router/install.sh

# Copy project-level agent templates to current project
bash agent-router/install.sh --init-project

# Uninstall
bash agent-router/install.sh --uninstall
```

## Architecture

### Agent Router Skill (v2.0.1)

The primary skill — provides automatic and manual routing to specialized sub-agents.

**Components:**

| File | Installed To | Purpose |
|------|--------------|---------|
| `agent-routing.md` | `~/.claude/rules/` | Always-on rule that routes tasks to appropriate agents |
| `agent-router.md` | `~/.claude/commands/` | Manual `/agent-router` command for force-dispatch |
| `agents/user/*.md` | `~/.claude/agents/` | User-level agent definitions (symlinked) |
| `agents/project-templates/*.md` | `.claude/agents/` | Project-level agents (copied on init) |

**Agent Types:**

- **User-level** (`agents/user/`): Shared across all projects, installed via symlink
  - `feature-planner.md` — Feature scoping and planning
  - `github-sync.md` — Git/GitHub operations
  - `local-dev-runner.md` — Build, test, environment ops

- **Project-level** (`agents/project-templates/`): Copied per-project, can be customized
  - `bug-finder-refiner.md` — Code quality audits (memory: project)
  - `ui-specialist.md` — UI/UX work (memory: project)

**Routing Priority:**

1. Tier 1 (immediate): `ui-specialist`, `bug-finder-refiner`, `github-sync`
2. Tier 2 (domain match): `local-dev-runner`, `feature-planner`, `Explore`, `Plan`
3. Main thread: trivial fixes, clarification questions

**State Tracking:**

Modifications are tracked in `~/.claude/projects/<project-key>/context-gardner-state.json` under the `agent_router_tracking` key. After 10+ modifications, suggests running `/context-gardner review`.

## Adding New Skills

Follow the existing pattern:
1. Create a subdirectory for the skill
2. Include `VERSION` file
3. Create `install.sh` that symlinks files to appropriate `~/.claude/` subdirectories
4. Update `skills.md` registry
