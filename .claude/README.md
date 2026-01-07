A growing library of custom Claude Code resources designed for real-world workflows.

## Philosophy

With a tool like Claude Code, the default assumption is simple: anything is possible—and it’s on you to act like it.

That’s the mindset these resources were built with.

## What’s Inside

**[Commands](#commands)** (27 total) — Slash commands that expand into structured workflows

* **Meta-prompting:** Separate planning from execution with staged prompts
* **Todo management:** Capture context mid-work, then resume later with full state
* **Thinking models:** Frameworks like first principles, inversion, 80/20, and more
* **Deep analysis:** Systematic debugging with evidence gathering and hypothesis testing

**[Skills](#skills)** (7 total) — Autonomous workflows that research, generate, and self-heal

* **Create Plans:** Hierarchical project planning for solo dev + Claude workflows
* **Create Agent Skills:** Build new skills by describing what you want
* **Create Meta-Prompts:** Generate staged workflow prompts with dependency detection
* **Create Slash Commands:** Build custom commands with proper structure
* **Create Subagents:** Create specialized Claude instances for isolated contexts
* **Create Hooks:** Build event-driven automation
* **Debug Like Expert:** Rigorous debugging with evidence and hypothesis testing

**[Agents](#agents)** (3 total) — Specialized subagents for validation and quality

* **skill-auditor:** Reviews skills for best-practices compliance
* **slash-command-auditor:** Reviews commands for proper structure
* **subagent-auditor:** Reviews agent configurations for effectiveness


## Commands

### Meta-Prompting

Separate analysis from execution: describe what you want in natural language, Claude generates a rigorous prompt, then runs it in a fresh sub-agent context.

* [`/create-prompt`](./commands/create-prompt.md) — Generate optimized prompts with XML structure
* [`/run-prompt`](./commands/run-prompt.md) — Execute saved prompts in sub-agent contexts

### Todo Management

Capture tasks without derailing the current thread—then pick up later with the original context intact.

* [`/add-to-todos`](./commands/add-to-todos.md) — Capture tasks with full context
* [`/check-todos`](./commands/check-todos.md) — Resume work on captured tasks

### Context Handoff

Generate a structured handoff you can reference later (via `@whats-next.md`) to continue seamlessly in a fresh context.

* [`/whats-next`](./commands/whats-next.md) — Create a handoff document

### Create Extensions

Wrapper commands that invoke the skills below.

* [`/create-agent-skill`](./commands/create-agent-skill.md) — Create a new skill
* [`/create-meta-prompt`](./commands/create-meta-prompt.md) — Create staged workflow prompts
* [`/create-slash-command`](./commands/create-slash-command.md) — Create a new slash command
* [`/create-subagent`](./commands/create-subagent.md) — Create a new subagent
* [`/create-hook`](./commands/create-hook.md) — Create a new hook

### Audit Extensions

Run auditor subagents.

* [`/audit-skill`](./commands/audit-skill.md) — Audit a skill for best practices
* [`/audit-slash-command`](./commands/audit-slash-command.md) — Audit a command for best practices
* [`/audit-subagent`](./commands/audit-subagent.md) — Audit a subagent for best practices

### Self-Improvement

* [`/heal-skill`](./commands/heal-skill.md) — Repair skills based on execution issues

### Thinking Models

Apply proven mental frameworks to decisions and problem-solving.

* [`/consider:pareto`](./commands/consider/pareto.md) — Apply the 80/20 rule
* [`/consider:first-principles`](./commands/consider/first-principles.md) — Rebuild from fundamentals
* [`/consider:inversion`](./commands/consider/inversion.md) — Solve backwards (what guarantees failure?)
* [`/consider:second-order`](./commands/consider/second-order.md) — Consider downstream effects
* [`/consider:5-whys`](./commands/consider/5-whys.md) — Drill to root cause
* [`/consider:occams-razor`](./commands/consider/occams-razor.md) — Prefer the simplest explanation
* [`/consider:one-thing`](./commands/consider/one-thing.md) — Find the highest-leverage action
* [`/consider:swot`](./commands/consider/swot.md) — Map strengths/weaknesses/opportunities/threats
* [`/consider:eisenhower-matrix`](./commands/consider/eisenhower-matrix.md) — Prioritize by urgent vs important
* [`/consider:10-10-10`](./commands/consider/10-10-10.md) — Evaluate across time horizons
* [`/consider:opportunity-cost`](./commands/consider/opportunity-cost.md) — Make tradeoffs explicit
* [`/consider:via-negativa`](./commands/consider/via-negativa.md) — Improve by removing

### Deep Analysis

Methodical debugging with structured investigation.

* [`/debug`](./commands/debug.md) — Apply expert debugging methodology

## Agents

Specialized subagents used by the audit commands.

* [`skill-auditor`](./agents/skill-auditor.md) — Best-practices skill auditor
* [`slash-command-auditor`](./agents/slash-command-auditor.md) — Slash command auditor
* [`subagent-auditor`](./agents/subagent-auditor.md) — Agent configuration auditor

## Skills

### [Create Plans](./skills/create-plans/)

Hierarchical project planning optimized for solo developer + Claude. The goal is executable plans Claude can run—not enterprise documentation that never gets used.

**PLAN.md is the prompt.** The workflow is: Brief → Roadmap → Research (if needed) → PLAN.md → Execute → SUMMARY.md.

**Domain-aware:** Optionally loads framework-specific expertise from `~/.claude/skills/expertise/` (e.g., macos-apps, iphone-apps) to produce concrete, framework-appropriate specs instead of generic ones. Domain expertise skills are created with [create-agent-skills](#create-agent-skills).

**Quality controls:** Research includes verification checklists, blind-spot reviews, critical-claim audits, and streaming writes to prevent gaps and token-limit failures.

**Context management:** Auto-handoff at ~10% tokens remaining. Git versioning commits outcomes, not process.

**Commands:** `/create-plan` (invoke skill), `/run-plan <path>` (execute PLAN.md with intelligent segmentation)

See the [create-plans README](./skills/create-plans/README.md) for full documentation.

### [Create Agent Skills](./skills/create-agent-skills/)

Generate skills by describing what you want. It asks clarifying questions, researches APIs if needed, and outputs properly structured skill files.

**Two skill types:**

1. **Task-execution skills** — Skills that perform specific operations
2. **Domain expertise skills** — Long-form knowledge bases (5k–10k+ lines) stored in `~/.claude/skills/expertise/` and loaded by other skills like [create-plans](#create-plans)

**Context-aware:** Detects when you’re already inside a skill directory and guides you through relevant options with progressive disclosure.

When something breaks, `/heal-skill` diagnoses what happened and updates the skill based on what actually worked.

Commands: `/create-agent-skill`, `/heal-skill`, `/audit-skill`

### [Create Meta-Prompts](./skills/create-meta-prompts/)

A skill-based evolution of the meta-prompting system. Produces prompts with structured outputs (research.md, plan.md) that later prompts can parse, plus dependency detection for chaining research → plan → implement.

**Note:** For end-to-end project work, prefer [create-plans](#create-plans). Use create-meta-prompts for abstract workflows and Claude→Claude pipelines; use create-plans to actually build projects with lifecycle management (brief → roadmap → execution → handoffs).

Commands: `/create-meta-prompt`

### [Create Slash Commands](./skills/create-slash-commands/)

Build commands that expand into full prompts when invoked. Describe what you want and get proper YAML configuration with arguments, tool restrictions, and dynamic context loading.

Commands: `/create-slash-command`, `/audit-slash-command`

### [Create Subagents](./skills/create-subagents/)

Create specialized Claude instances that run in isolated contexts. Describe the purpose and get an optimized system prompt with the right tool access and orchestration patterns.

Commands: `/create-subagent`, `/audit-subagent`

### [Create Hooks](./skills/create-hooks/)

Create event-driven automations that trigger on tool calls, session events, or prompt submissions. Describe what you want automated and get working hook configurations.

Commands: `/create-hook`

### [Debug Like Expert](./skills/debug-like-expert/)

Deep-analysis debugging for complex issues. Uses a structured investigation protocol: evidence gathering, hypothesis testing, and verification. Best for when standard troubleshooting fails.

Commands: `/debug`

---

## Recommended Workflow

**For building projects:** Use `/create-plan` to invoke [create-plans](#create-plans). Then run `/run-plan <path-to-PLAN.md>` to execute phases with intelligent segmentation. You’ll get hierarchical planning (BRIEF.md → ROADMAP.md → phases/PLAN.md), domain-aware task generation, context handoffs, and outcome-focused git versioning.

**For domain expertise:** Use [create-agent-skills](#create-agent-skills) to generate knowledge bases in `~/.claude/skills/expertise/`. create-plans will automatically load them to produce framework-specific tasks instead of generic ones.

**For everything else:** Use [create-meta-prompts](#create-meta-prompts-1) or `/create-prompt` + `/run-prompt` for Claude→Claude pipelines that don’t fit the project-planning lifecycle.

---
