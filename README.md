# Next.js Project Template with Claude Code Configuration

A reusable project template for Next.js applications, pre-configured with Claude Code skills, commands, and agents.

## What's Included

### `.claude/` Directory

The complete Claude Code configuration with:

- **skills/**: Reusable skill definitions for common tasks
  - `create-slash-commands/` - Create custom slash commands
  - `create-meta-prompts/` - Build Claude-to-Claude pipelines
  - `create-agent-skills/` - Author new skills
  - `create-hooks/` - Configure event listeners
  - `debug-like-expert/` - Systematic debugging methodology
  - `create-subagents/` - Build specialized agents
  - `create-plans/` - Hierarchical project planning
  - `expertise/` - Domain-specific knowledge (macOS, iOS apps)

- **commands/**: Slash commands for common workflows
  - Audit commands (`/audit-skill`, `/audit-slash-command`, `/audit-subagent`)
  - Creation commands (`/create-plan`, `/create-prompt`, `/create-subagent`, etc.)
  - Utility commands (`/check-todos`, `/debug`, `/whats-next`)
  - Mental models (`/consider/*` - various thinking frameworks)

- **agents/**: Specialized subagent configurations
  - `skill-auditor.md` - Review skill quality
  - `slash-command-auditor.md` - Review command quality
  - `subagent-auditor.md` - Review subagent quality

- **docs/**: Reference documentation
  - Meta-prompting patterns
  - Todo management
  - Context handoff strategies

### `CLAUDE.md`

A two-tier project documentation file:

1. **Core Section** (~30 lines): Essential technical stack, architecture, file structure, and development commands
2. **Reference Patterns Section**: Optional patterns for animations, design systems, and image handling - delete what you don't need

## How to Use

### 1. Copy to Your Project

```bash
# Copy the .claude folder
cp -r templates/.claude /path/to/your/project/

# Copy and customize CLAUDE.md
cp templates/CLAUDE.md /path/to/your/project/CLAUDE.md
```

### 2. Customize CLAUDE.md

Open your new `CLAUDE.md` and:

1. Replace `[Project Name]` with your actual project name
2. Update framework versions if different from defaults
3. Review the Reference Patterns section:
   - Keep patterns relevant to your project
   - Delete entire sections you don't need
   - Fill in placeholder values (colors, fonts, etc.)

### 3. Verify Setup

Test that Claude Code recognizes your configuration:

```bash
# Run any slash command to verify
claude /check-todos
```

## Template Structure

```
templates/
  .claude/
    skills/          # Skill definitions
    commands/        # Slash commands
    agents/          # Subagent configurations
    docs/            # Reference docs
    README.md        # Claude Code plugin info
    LICENSE          # MIT License
  CLAUDE.md          # Project documentation template
  README.md          # This file
```

## Core vs Optional Sections

### Core (Always Keep)

- Technical Stack
- Architecture
- File Structure
- Development commands

### Optional (Delete If Not Needed)

- Animation Patterns - only if using Framer Motion
- Design System - only if defining custom tokens
- Image Handling - only if using next/image extensively

## Updating Framework Versions

The template specifies:
- Next.js 16.1.1
- Tailwind CSS 4.1.18

To update, modify the versions in `CLAUDE.md` to match your `package.json`.

## License

MIT License - see `.claude/LICENSE` for details.
