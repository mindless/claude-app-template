# Next.js Project Template with Claude Code Configuration

A reusable project template for Next.js applications, pre-configured with Claude Code skills, commands, and agents.

## Philosophy

With a tool like Claude Code, the default assumption is simple: anything is possible—and it's on you to act like it.

That's the mindset these resources were built with.

## What's Included

### `.claude/` Directory

The complete Claude Code configuration with:

**Skills** (7 total) — Autonomous workflows that research, generate, and self-heal

- **Create Plans:** Hierarchical project planning for solo dev + Claude workflows
- **Create Agent Skills:** Build new skills by describing what you want
- **Create Meta-Prompts:** Generate staged workflow prompts with dependency detection
- **Create Slash Commands:** Build custom commands with proper structure
- **Create Subagents:** Create specialized Claude instances for isolated contexts
- **Create Hooks:** Build event-driven automation
- **Debug Like Expert:** Rigorous debugging with evidence and hypothesis testing
- **Domain Expertise:** Framework-specific knowledge (macOS, iOS apps)

**Commands** (27 total) — Slash commands that expand into structured workflows

- **Meta-prompting:** `/create-prompt`, `/run-prompt`
- **Todo management:** `/add-to-todos`, `/check-todos`
- **Context handoff:** `/whats-next`
- **Creation:** `/create-plan`, `/create-agent-skill`, `/create-slash-command`, `/create-subagent`, `/create-hook`
- **Auditing:** `/audit-skill`, `/audit-slash-command`, `/audit-subagent`
- **Thinking models:** `/consider:pareto`, `/consider:first-principles`, `/consider:inversion`, and more
- **Deep analysis:** `/debug`

**Agents** (3 total) — Specialized subagents for validation and quality

- **skill-auditor:** Reviews skills for best-practices compliance
- **slash-command-auditor:** Reviews commands for proper structure
- **subagent-auditor:** Reviews agent configurations for effectiveness

### `CLAUDE.md`

A two-tier project documentation file:

1. **Core Section** (~30 lines): Essential technical stack, architecture, file structure, and development commands
2. **Reference Patterns Section**: Optional patterns for animations, design systems, and image handling - delete what you don't need

## How to Use

### 1. Clone or Copy to Your Project

```bash
# Clone the template
git clone https://github.com/mindless/claude-app-template.git my-project

# Or copy to an existing project
cp -r claude-app-template/.claude /path/to/your/project/
cp claude-app-template/CLAUDE.md /path/to/your/project/
```

### 2. Customize CLAUDE.md

Open your `CLAUDE.md` and:

1. Replace `[Project Name]` with your actual project name
2. Update framework versions if different from defaults
3. Review the Reference Patterns section:
   - Keep patterns relevant to your project
   - Delete entire sections you don't need
   - Fill in placeholder values (colors, fonts, etc.)

### 3. Verify Setup

Test that Claude Code recognizes your configuration:

```bash
claude /check-todos
```

## Recommended Workflows

**For building projects:** Use `/create-plan` to invoke hierarchical planning (BRIEF.md → ROADMAP.md → phases/PLAN.md). Then run `/run-plan <path-to-PLAN.md>` to execute phases with intelligent segmentation.

**For domain expertise:** Use `/create-agent-skill` to generate knowledge bases in `~/.claude/skills/expertise/`. The create-plans skill will automatically load them to produce framework-specific tasks.

**For meta-prompting:** Use `/create-prompt` + `/run-prompt` for Claude-to-Claude pipelines that don't fit the project-planning lifecycle.

## Skills Reference

### Create Plans

Hierarchical project planning optimized for solo developer + Claude. The goal is executable plans Claude can run—not enterprise documentation.

**PLAN.md is the prompt.** The workflow is: Brief → Roadmap → Research (if needed) → PLAN.md → Execute → SUMMARY.md.

**Domain-aware:** Optionally loads framework-specific expertise from `~/.claude/skills/expertise/` (e.g., macos-apps, iphone-apps).

**Quality controls:** Research includes verification checklists, blind-spot reviews, critical-claim audits.

**Context management:** Auto-handoff at ~10% tokens remaining. Git versioning commits outcomes, not process.

### Create Agent Skills

Generate skills by describing what you want. It asks clarifying questions, researches APIs if needed, and outputs properly structured skill files.

**Two skill types:**
1. **Task-execution skills** — Skills that perform specific operations
2. **Domain expertise skills** — Long-form knowledge bases (5k–10k+ lines) stored in `~/.claude/skills/expertise/`

When something breaks, `/heal-skill` diagnoses what happened and updates the skill based on what actually worked.

### Create Meta-Prompts

Produces prompts with structured outputs (research.md, plan.md) that later prompts can parse, plus dependency detection for chaining research → plan → implement.

### Create Slash Commands

Build commands that expand into full prompts when invoked. Describe what you want and get proper YAML configuration with arguments, tool restrictions, and dynamic context loading.

### Create Subagents

Create specialized Claude instances that run in isolated contexts. Describe the purpose and get an optimized system prompt with the right tool access and orchestration patterns.

### Create Hooks

Create event-driven automations that trigger on tool calls, session events, or prompt submissions.

### Debug Like Expert

Deep-analysis debugging for complex issues. Uses a structured investigation protocol: evidence gathering, hypothesis testing, and verification.

## Commands Reference

### Meta-Prompting

- `/create-prompt` — Generate optimized prompts with XML structure
- `/run-prompt` — Execute saved prompts in sub-agent contexts

### Todo Management

- `/add-to-todos` — Capture tasks with full context
- `/check-todos` — Resume work on captured tasks

### Context Handoff

- `/whats-next` — Create a handoff document for continuing in a fresh context

### Thinking Models

- `/consider:pareto` — Apply the 80/20 rule
- `/consider:first-principles` — Rebuild from fundamentals
- `/consider:inversion` — Solve backwards (what guarantees failure?)
- `/consider:second-order` — Consider downstream effects
- `/consider:5-whys` — Drill to root cause
- `/consider:occams-razor` — Prefer the simplest explanation
- `/consider:one-thing` — Find the highest-leverage action
- `/consider:swot` — Map strengths/weaknesses/opportunities/threats
- `/consider:eisenhower-matrix` — Prioritize by urgent vs important
- `/consider:10-10-10` — Evaluate across time horizons
- `/consider:opportunity-cost` — Make tradeoffs explicit
- `/consider:via-negativa` — Improve by removing

### Deep Analysis

- `/debug` — Apply expert debugging methodology

## Project Structure

```
.claude/
  skills/          # Skill definitions
  commands/        # Slash commands
  agents/          # Subagent configurations
  docs/            # Reference docs
  LICENSE          # MIT License
CLAUDE.md          # Project documentation template
README.md          # This file
```

## License

MIT License - see `.claude/LICENSE` for details.
