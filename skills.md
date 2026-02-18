# Skills Registry

Custom skills installed for Claude Code. Each skill lives in its own subdirectory of this directory and is symlinked into `~/.claude/` via its `install.sh`.

## Installed Skills

### Agent Router (v2.0.1)
- **Directory:** `agent-router/`
- **Purpose:** Automatic and manual sub-agent dispatch. Routes every task to the best specialized agent based on keyword signals and task intent. Ships agent definitions alongside routing logic.
- **Components:**
  - `agent-routing.md` — Always-on rule (loaded into every conversation via `~/.claude/rules/`)
  - `agent-router.md` — Manual `/agent-router` command (via `~/.claude/commands/`)
  - `VERSION` — Version tracking
  - `install.sh` — Installer (supports `--uninstall` and `--init-project`)
  - `agents/user/` — User-level agent definitions (symlinked to `~/.claude/agents/`):
    - `feature-planner.md` — Feature scoping and planning specialist
    - `github-sync.md` — Git/GitHub operations specialist
    - `local-dev-runner.md` — Build, test, and environment operations
  - `agents/project-templates/` — Project-level agent templates (copied on demand):
    - `bug-finder-refiner.md` — Code quality auditor (memory: project)
    - `ui-specialist.md` — UI/UX engineer (memory: project)
- **Usage:**
  - Automatic: Just describe your task — routing happens transparently
  - Manual: `/agent-router <agent-name> <task>` to force-dispatch (case-insensitive)
  - Status: `/agent-router status` to see tracked modifications
  - Version: `/agent-router version`
- **Install:**
  - `bash install.sh` — Install rule, command, and user-level agents
  - `bash install.sh --init-project` — Copy project templates into `.claude/agents/`
  - `bash install.sh --uninstall` — Remove all installed symlinks
- **Integration:** Tracks modifications in project-level `context-gardner-state.json` under the `agent_router_tracking` key. Suggests `/context-gardner review` after 10+ modifications since last review.

### Context Gardner (v1.0.0)
- **Directory:** `context-gardner/`
- **Purpose:** Memory file management and session persistence. Review, prune, move, and pin entries across CLAUDE.md and auto-memory files. Includes a session-resume rule for autonomous working memory.
- **Components:**
  - `session-resume.md` — Always-on rule (loaded into every conversation via `~/.claude/rules/`)
  - `context-gardner.md` — Dispatcher command (via `~/.claude/commands/`)
  - `checkpoint.md` — Capture session context before it's lost
  - `review-memory.md` — Autonomous review with proposal + approval
  - `prune-memory.md` — Automated pruning with approval
  - `move-memory.md` — Move sections between memory files
  - `overflow.md` — Move detailed MEMORY.md sections to topic files
  - `pin.md` — Pin entries to protect from pruning
  - `restore.md` — List and restore pre-modification snapshots
  - `VERSION` — Version tracking
  - `install.sh` — Installer (supports `--uninstall`)
- **Usage:** `/context-gardner <subcommand> [arguments]`
- **Install:**
  - `bash context-gardner/install.sh` — Install rule and all commands
  - `bash context-gardner/install.sh --uninstall` — Remove all installed symlinks
- **Integration:** Agent Router tracks modifications in `context-gardner-state.json` and suggests `/context-gardner review` after 10+ changes.
