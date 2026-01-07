<objective>
Expand the Philosophy section in README.md to comprehensively cover metaprompting philosophy and explain why the tools in this template are configured the way they are.

The goal is to help users understand the mental model behind this template—not just what's included, but why it exists and how to think about AI-assisted development.
</objective>

<context>
@README.md — The current README with a brief 2-line philosophy section that needs expansion
@.claude/ — The configuration directory containing skills, commands, and agents

This is a Claude Code configuration template. Users clone it to get pre-built skills, commands, and agents for AI-assisted development workflows.

The current Philosophy section says only:
> With a tool like Claude Code, the default assumption is simple: anything is possible—and it's on you to act like it.

This needs to become a substantive section explaining the principles behind the template.
</context>

<requirements>
Expand the Philosophy section to cover these concepts in an educational and practical tone:

**1. Metaprompting Philosophy**

Explain these core principles:

- **Prompt-as-code**: Prompts are software artifacts. They should be versioned, iterated, and treated with the same rigor as code. The `/create-prompt` command exists because prompts deserve the same lifecycle management as any other codebase artifact.

- **Claude-to-Claude pipelines**: Prompts can generate structured outputs that become inputs for other prompts. The pattern is research → plan → implement. Each stage produces artifacts (research.md, PLAN.md) that the next stage consumes. This is why `/create-meta-prompt` has dependency detection built in.

- **Context isolation**: Fresh contexts matter. When prompts run via `/run-prompt`, each agent operates in its own git worktree. This enables safe parallel execution without merge conflicts, clean atomic merges back to main, and prevents context pollution between tasks.

**2. Why Hierarchical Planning**

Explain the rationale for the BRIEF → ROADMAP → PLAN → Execute flow:

- **PLAN.md is the prompt**: The plan isn't documentation—it's the executable instruction set Claude will follow. Writing a plan IS writing a prompt for future Claude instances.

- **Progressive refinement**: Brief captures intent, Roadmap structures phases, Research fills knowledge gaps, PLAN provides executable steps. Each layer adds specificity without losing the big picture.

- **Why not just start coding?**: Because Claude works best with clear, scoped objectives. A well-structured plan prevents scope creep, ensures nothing is forgotten, and makes handoffs between sessions seamless.

**3. The Separation: Skills, Commands, Agents**

Briefly explain why these are distinct concepts:
- **Skills**: Domain knowledge and workflows (the "how")
- **Commands**: Entry points and shortcuts (the "trigger")
- **Agents**: Isolated execution contexts (the "where")

**Tone Guidelines**
- Educational but practical—explain concepts, then show what it means for actual use
- Avoid jargon without explanation
- Use concrete examples from the template itself
- Keep it scannable with clear headers
</requirements>

<constraints>
- Keep the existing opening lines about "anything is possible" — expand around them, don't replace them
- The Philosophy section should come before "What's Included" (current position is correct)
- Aim for ~40-60 lines of content (substantial but not overwhelming)
- Use markdown formatting consistent with the rest of the README
- Don't duplicate information already covered in other sections—reference them instead
</constraints>

<output>
Modify `./README.md` to replace the current Philosophy section with the expanded version.

The section should have this structure:
```
## Philosophy

[Existing opening lines preserved]

### Metaprompting as a Discipline
[Prompt-as-code, Claude-to-Claude pipelines, context isolation]

### Why Hierarchical Planning
[PLAN.md is the prompt, progressive refinement, why not just code]

### Skills, Commands, and Agents
[Brief explanation of separation of concerns]
```
</output>

<verification>
Before completing:
1. Verify the opening lines are preserved
2. Confirm all three subsections are present and substantive
3. Check that the tone is educational/practical, not academic or preachy
4. Ensure no content from other README sections is duplicated
5. Read through once to confirm it flows naturally
</verification>

<success_criteria>
- Philosophy section expanded from 2 lines to ~40-60 lines
- All three conceptual areas covered (metaprompting, hierarchical planning, separation)
- Existing opening preserved and enhanced
- Practical examples reference actual template components
- A new user reading this section understands WHY the template exists, not just what it contains
</success_criteria>
