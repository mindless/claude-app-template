# Next.js Project Template with Claude Code Configuration

A reusable project template for Next.js applications, pre-configured with Claude Code skills, commands, and agents.

## Philosophy

With a tool like Claude Code, the default assumption is simple: anything is possible—and it's on you to act like it.

That's the mindset these resources were built with. But "anything is possible" doesn't mean "do anything randomly." The tools in this template encode specific principles about how to work effectively with AI—principles that emerge from treating AI-assisted development as a discipline rather than an improvisation.

### Metaprompting as a Discipline

**Prompts are code.** A prompt isn't throwaway text you type and forget—it's a software artifact. It should be versioned, tested, iterated, and refined like any other piece of your codebase. That's why `/create-prompt` exists: because prompts deserve the same lifecycle management as the code they help produce.

**Claude-to-Claude pipelines.** A single prompt rarely captures a complete workflow. Instead, prompts can generate structured outputs that become inputs for other prompts. The canonical pattern is: research fills knowledge gaps and produces `research.md`, planning consumes that research and produces `PLAN.md`, execution follows the plan. Each stage creates artifacts the next stage parses. This is why `/create-meta-prompt` includes dependency detection—it's designed for chained workflows where outputs flow into inputs.

**Context isolation matters.** Fresh contexts prevent accumulated confusion. When prompts run via `/run-prompt`, each agent operates in its own git worktree. This enables safe parallel execution (multiple prompts can run simultaneously without merge conflicts), clean atomic merges back to main, and prevents context pollution between unrelated tasks. Isolation isn't overhead—it's what makes complex workflows reliable.

### Why Hierarchical Planning

The template uses a BRIEF, ROADMAP, PLAN, Execute flow. Here's why:

**PLAN.md is the prompt.** A plan file isn't documentation for humans to skim—it's the executable instruction set Claude will follow. When you write a PLAN.md, you're writing a prompt for future Claude instances. This reframes planning: the quality of your plan directly determines the quality of execution.

**Progressive refinement, not big design up front.** A Brief captures intent without overspecifying. A Roadmap structures that intent into phases. Research fills knowledge gaps for the current phase. A PLAN provides executable steps with verification criteria. Each layer adds specificity without losing the big picture. You don't need perfect foresight—you refine as you go.

**Why not just start coding?** Because Claude works best with clear, scoped objectives. A well-structured plan prevents scope creep mid-task, ensures nothing is forgotten between sessions, and makes handoffs seamless when you hit context limits. The upfront investment in planning pays dividends in execution quality. See the Create Plans skill for the full workflow.

### Skills, Commands, and Agents

The template separates these three concepts deliberately:

**Skills** are domain knowledge and workflows—the "how." A skill teaches Claude how to accomplish something: how to create plans, how to debug systematically, how to generate other skills. Skills contain the methodology.

**Commands** are entry points and shortcuts—the "trigger." A command is a convenient way to invoke a skill or workflow. `/create-plan` is easier to type than explaining what you want. Commands are the user interface to skills.

**Agents** are isolated execution contexts—the "where." When a task needs to run without polluting your main context, an agent provides a clean environment with its own system prompt and tool access. Agents are the containers that skills run inside.

This separation lets you mix and match: invoke skills directly, trigger them via commands, or orchestrate them through agents—depending on what the task requires.

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

This template is designed to drop into any project. Most users add it to an existing codebase rather than starting fresh.

### Adding to an Existing Project

Copy the `.claude/` directory and `CLAUDE.md` to your project root:

```bash
# From inside the template directory
cp -r .claude /path/to/your/project/
cp CLAUDE.md /path/to/your/project/

# Or from anywhere
git clone https://github.com/mindless/claude-app-template.git /tmp/claude-template
cp -r /tmp/claude-template/.claude /path/to/your/project/
cp /tmp/claude-template/CLAUDE.md /path/to/your/project/
```

**What you're copying:**
- `.claude/` — Skills, commands, and agents that extend Claude Code
- `CLAUDE.md` — Project documentation template for Claude to read

### Starting a New Project

If you're starting fresh, clone the entire template and build from there:

```bash
git clone https://github.com/mindless/claude-app-template.git my-project
cd my-project
rm -rf .git && git init  # Start with a clean git history
```

### Customizing CLAUDE.md

Open your `CLAUDE.md` and:

1. Replace `[Project Name]` with your actual project name
2. Update the technical stack to match your project (framework, language, styling, etc.)
3. Update the file structure to reflect your actual directory layout
4. Review the Reference Patterns section:
   - Keep patterns relevant to your project
   - Delete entire sections you don't need
   - Fill in placeholder values (colors, fonts, etc.)

### Handling Conflicts

**Already have a `CLAUDE.md`?**
Merge the content. Keep your existing project-specific documentation and add any useful patterns from the template's Reference Patterns section.

**Already have a `.claude/` directory?**
Merge the directories. Your existing configurations take precedence. Copy only the skills, commands, or agents you want:

```bash
# Copy specific skills
cp -r template/.claude/skills/create-plans.md your-project/.claude/skills/

# Copy specific commands
cp -r template/.claude/commands/create-plan.md your-project/.claude/commands/
```

**Want only specific features?**
You don't need everything. Pick what's useful:
- Just want planning? Copy `skills/create-plans.md` and `commands/create-plan.md`
- Just want thinking models? Copy `commands/consider:*.md`
- Just want meta-prompting? Copy the `create-prompt` and `run-prompt` skills and commands

### Verify Setup

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
