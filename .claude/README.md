# Conductor for Claude Code

Context-driven development for AI coding assistants. **Measure twice, code once.**

Conductor helps you plan before you build - creating specs, implementation plans, and tracking progress through "tracks" (features, bugs, improvements).

## Installation

### Option 1: Claude Code Plugin (Recommended)

```bash
# Add the marketplace
/plugin marketplace add gemini-cli-extensions/conductor

# Install the plugin
/plugin install conductor

# Verify installation
/help
```

This installs:
- **16 slash commands** for direct invocation
- **1 skill** that auto-activates for conductor projects

### Option 2: Agent Skills Compatible CLI

If your CLI supports the [Agent Skills specification](https://agentskills.io):

```bash
# Point to the skill directory
skills/conductor/
├── SKILL.md                    # Entry point - overview, intent mapping, command routing
└── references/
    ├── workflows.md            # Workflow overview, state files, Beads & parallel overview
    ├── structure.md            # Directory structure reference
    ├── beads-integration.md    # Beads session protocol, CLI commands, chemistry
    ├── learnings-system.md     # Ralph-style knowledge capture
    ├── patterns-template.md    # Template for conductor/patterns.md
    ├── learnings-template.md   # Template for track learnings.md
    └── commands/               # Full step-by-step protocols for all 16 commands
        ├── setup.md
        ├── newtrack.md
        ├── implement.md
        ├── status.md
        ├── revert.md
        ├── validate.md
        ├── block.md
        ├── skip.md
        ├── revise.md
        ├── archive.md
        ├── export.md
        ├── handoff.md
        ├── refresh.md
        ├── formula.md
        ├── wisp.md
        └── distill.md
```

The skill follows the Agent Skills spec with full frontmatter:
- `name`: conductor
- `description`: Context-driven development methodology
- `license`: Apache-2.0
- `compatibility`: Claude Code, Gemini CLI, any Agent Skills compatible CLI
- `metadata`: version, author, repository, keywords

### Option 3: Manual Installation

Copy to your project:
```bash
cp -r /path/to/conductor/.claude your-project/
```

Or for global access (all projects):
```bash
cp -r /path/to/conductor/.claude/commands/* ~/.claude/commands/
cp -r /path/to/conductor/.claude/skills/* ~/.claude/skills/
```

### Option 4: Gemini CLI

If using Gemini CLI instead of Claude Code:
```bash
gemini extensions install https://github.com/gemini-cli-extensions/conductor
```

## Commands

| Command | Description |
|---------|-------------|
| `/conductor-setup` | Initialize project with product.md, tech-stack.md, workflow.md |
| `/conductor-newtrack [desc]` | Create new feature/bug track with spec and plan |
| `/conductor-implement [id]` | Execute tasks from track's plan (TDD workflow) |
| `/conductor-status` | Display progress overview |
| `/conductor-revert` | Git-aware revert of tracks, phases, or tasks |
| `/conductor-validate` | Validate project integrity |
| `/conductor-block` | Mark task as blocked with reason |
| `/conductor-skip` | Skip current task with justification |
| `/conductor-revise` | Update spec/plan when issues found |
| `/conductor-archive` | Archive completed tracks |
| `/conductor-export` | Export project summary |
| `/conductor-handoff` | Create context handoff for session transfer |
| `/conductor-refresh` | Sync context docs with codebase state |
| `/conductor-formula` | List and manage track templates (Beads formulas) |
| `/conductor-wisp` | Create ephemeral exploration track (no audit trail) |
| `/conductor-distill` | Extract reusable template from completed track |

## Skill (Auto-Activation)

The conductor skill automatically activates when Claude detects:
- A `conductor/` directory in the project
- References to tracks, specs, plans
- Context-driven development keywords

You can also use natural language:
- "Help me plan the authentication feature"
- "What's the current project status?"
- "Set up this project with Conductor"
- "Create a spec for the dark mode feature"

## How It Works

### 1. Setup
Run `/conductor-setup` to initialize your project with:
```
conductor/
├── product.md           # What you're building and for whom
├── tech-stack.md        # Technology choices and constraints
├── workflow.md          # Development standards (TDD, commits)
├── tracks.md            # Master list of all work items
├── patterns.md          # Consolidated learnings (Ralph-style)
└── beads.json           # Beads integration config
```

### 2. Create Tracks
Run `/conductor-newtrack "Add user authentication"` to create:
```
conductor/tracks/auth_20241219/
├── metadata.json        # Track type, status, dates, priority
├── spec.md              # Requirements and acceptance criteria
├── plan.md              # Phased implementation plan
└── learnings.md         # Patterns/gotchas discovered
```

### 3. Implement
Run `/conductor-implement` to execute the plan:
- Follows TDD: Write tests → Implement → Refactor
- Commits after each task with conventional messages
- Updates plan.md with progress and commit SHAs
- Captures learnings after each task
- Verifies at phase completion

### 4. Track Progress
Run `/conductor-status` to see:
- Overall project progress with priority grouping
- Current active track and task
- Parallel worker status (if active)
- Beads task status (if enabled)
- Next actions needed

## Status Markers

Throughout conductor files:
- `[ ]` - Pending/New
- `[~]` - In Progress
- `[x]` - Completed (with commit SHA)
- `[!]` - Blocked (followed by reason)
- `[-]` - Skipped (followed by reason)

## Gemini CLI Interoperability

Projects work with both Gemini CLI and Claude Code:

| Gemini CLI | Claude Code |
|------------|-------------|
| `/conductor:setup` | `/conductor-setup` |
| `/conductor:newTrack` | `/conductor-newtrack` |
| `/conductor:implement` | `/conductor-implement` |
| `/conductor:status` | `/conductor-status` |
| `/conductor:revert` | `/conductor-revert` |
| `/conductor:validate` | `/conductor-validate` |
| `/conductor:block` | `/conductor-block` |
| `/conductor:skip` | `/conductor-skip` |
| `/conductor:revise` | `/conductor-revise` |
| `/conductor:archive` | `/conductor-archive` |
| `/conductor:export` | `/conductor-export` |
| `/conductor:handoff` | `/conductor-handoff` |
| `/conductor:refresh` | `/conductor-refresh` |
| `/conductor:formula` | `/conductor-formula` |
| `/conductor:wisp` | `/conductor-wisp` |
| `/conductor:distill` | `/conductor-distill` |

Same `conductor/` directory structure, full compatibility.

## File Structure

```
.claude/
├── commands/                     # Claude Code slash commands (16)
│   ├── conductor-setup.md
│   ├── conductor-newtrack.md
│   ├── conductor-implement.md
│   ├── conductor-status.md
│   ├── conductor-revert.md
│   ├── conductor-validate.md
│   ├── conductor-block.md
│   ├── conductor-skip.md
│   ├── conductor-revise.md
│   ├── conductor-archive.md
│   ├── conductor-export.md
│   ├── conductor-handoff.md
│   ├── conductor-refresh.md
│   ├── conductor-formula.md
│   ├── conductor-wisp.md
│   └── conductor-distill.md
├── skills/
│   ├── conductor/                # Context-driven development skill
│   │   ├── SKILL.md              # Entry point (overview, intent mapping, command routing)
│   │   └── references/
│   │       ├── workflows.md      # Workflow overview, state files, Beads & parallel overview
│   │       ├── structure.md      # Directory structure reference
│   │       ├── beads-integration.md
│   │       ├── learnings-system.md
│   │       ├── patterns-template.md
│   │       ├── learnings-template.md
│   │       └── commands/         # Full protocols (what agents read to execute commands)
│   │           ├── setup.md      # (397 lines)
│   │           ├── newtrack.md   # (390 lines)
│   │           ├── implement.md  # (566 lines)
│   │           ├── status.md     # (178 lines)
│   │           ├── revert.md     # (196 lines)
│   │           ├── validate.md   # (92 lines)
│   │           ├── block.md      # (48 lines)
│   │           ├── skip.md       # (59 lines)
│   │           ├── revise.md     # (155 lines)
│   │           ├── archive.md    # (97 lines)
│   │           ├── export.md     # (51 lines)
│   │           ├── handoff.md    # (193 lines)
│   │           ├── refresh.md    # (141 lines)
│   │           ├── formula.md    # (156 lines)
│   │           ├── wisp.md       # (214 lines)
│   │           └── distill.md    # (242 lines)
│   ├── beads/                    # Persistent task memory skill
│   └── skill-creator/            # Skill development guide
└── README.md                     # This file
```

## Links

- [GitHub Repository](https://github.com/gemini-cli-extensions/conductor)
- [Agent Skills Specification](https://agentskills.io)
- [Gemini CLI Extensions](https://geminicli.com/docs/extensions/)

## License

Apache-2.0
