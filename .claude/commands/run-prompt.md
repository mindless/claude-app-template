---
name: run-prompt
description: Delegate one or more prompts to fresh sub-task contexts with parallel or sequential execution
argument-hint: <prompt-number(s)-or-name> [--parallel|--sequential|--auto]
allowed-tools: [Read, Task, Bash, Skill]
---

<context>
Git status: !`git status --short`
Current branch: !`git branch --show-current`
Recent prompts: !`ls -t ./prompts/*.md 2>/dev/null | head -5`
</context>

<objective>
Execute one or more prompts from `./prompts/` as delegated sub-tasks. Each agent runs in an isolated git worktree for safe parallel execution, then cleanly merges back and cleans up.

Key features:
- **Worktree isolation**: Each agent gets its own git worktree (no merge conflicts)
- **Auto execution strategy**: Analyzes prompt metadata to determine parallel vs sequential
- **Frontend-aware**: Automatically uses frontend-design skill + Playwright MCP for UI tasks
- **Clean merge & cleanup**: Merges work back to main branch and removes worktree
</objective>

<input>
The user will specify which prompt(s) to run via $ARGUMENTS, which can be:

**Single prompt:**

- Empty (no arguments): Run the most recently created prompt (default behavior)
- A prompt number (e.g., "001", "5", "42")
- A partial filename (e.g., "user-auth", "dashboard")

**Multiple prompts:**

- Multiple numbers (e.g., "005 006 007")
- With execution flag: "005 006 007 --parallel" or "005 006 007 --sequential"
- With auto-detection: "005 006 007 --auto" (analyzes prompt metadata for dependencies)
- If no flag specified with multiple prompts, default to --auto (smart detection)
</input>

<process>
<step1_parse_arguments>
Parse $ARGUMENTS to extract:
- Prompt numbers/names (all arguments that are not flags)
- Execution strategy flag (--parallel or --sequential)

<examples>
- "005" → Single prompt: 005
- "005 006 007" → Multiple prompts: [005, 006, 007], strategy: auto (analyze metadata)
- "005 006 007 --parallel" → Multiple prompts: [005, 006, 007], strategy: parallel (forced)
- "005 006 007 --sequential" → Multiple prompts: [005, 006, 007], strategy: sequential (forced)
- "005 006 007 --auto" → Multiple prompts: [005, 006, 007], strategy: auto (explicit)
</examples>
</step1_parse_arguments>

<step1b_analyze_prompts>
For each resolved prompt file, analyze its content:

1. **Check for execution metadata** (at top of file):
   ```
   <!-- execution-metadata
   parallel-safe: true|false
   depends-on: [list of prompt numbers]
   touches-files: [list of files/directories]
   -->
   ```

2. **Detect frontend tasks** by scanning for:
   - `<frontend_tooling>` tag in prompt
   - Keywords: component, UI, CSS, Tailwind, layout, animation, visual, Framer Motion
   - File references to `.tsx`, `.css`, `components/`, `styles/`

3. **Build dependency graph** (for --auto mode):
   - If prompt A's `touches-files` overlaps with prompt B's → sequential
   - If prompt has `depends-on` referencing another prompt → sequential
   - If `parallel-safe: true` and no overlapping files → parallel
   - Default: sequential (safer)

<auto_strategy_decision>
Given prompts [A, B, C]:

**Run in PARALLEL if ALL of:**
- All prompts have `parallel-safe: true` OR no overlapping `touches-files`
- No `depends-on` relationships between them
- No prompts modify the same directories

**Run SEQUENTIALLY if ANY of:**
- Any prompt has `depends-on` pointing to another
- Overlapping `touches-files` detected
- Any prompt has `parallel-safe: false`

**Mixed execution** (advanced):
- If A and B can be parallel but C depends on both → run A+B parallel, then C
</auto_strategy_decision>
</step1b_analyze_prompts>

<step1c_detect_frontend>
For each prompt, if frontend task detected:

1. **Prepend frontend instructions** to the prompt content when delegating:
   ```
   FRONTEND TASK DETECTED - Use this workflow:
   1. First, invoke /frontend-design skill to get design guidance
   2. Use Playwright MCP for visual reconnaissance before changes
   3. Implement changes following the design skill's output
   4. Use Playwright MCP to capture screenshots after changes
   5. Validate visually and iterate if needed
   ```

2. **Ensure agent has access** to:
   - Skill tool for `/frontend-design`
   - Playwright MCP for screenshots and visual validation
</step1c_detect_frontend>

<step2_resolve_files>
For each prompt number/name:

- If empty or "last": Find with `!ls -t ./prompts/*.md | head -1`
- If a number: Find file matching that zero-padded number (e.g., "5" matches "005-_.md", "42" matches "042-_.md")
- If text: Find files containing that string in the filename

<matching_rules>

- If exactly one match found: Use that file
- If multiple matches found: List them and ask user to choose
- If no matches found: Report error and list available prompts
  </matching_rules>
  </step2_resolve_files>

<step3_execute>

<worktree_setup>
**CRITICAL: Each agent runs in its own isolated git worktree**

For each prompt to execute, create a worktree:
```bash
# Generate unique branch name
BRANCH_NAME="prompt-$(prompt_number)-$(date +%s)"
WORKTREE_PATH="../.worktrees/$BRANCH_NAME"

# Create worktree with new branch from current HEAD
git worktree add "$WORKTREE_PATH" -b "$BRANCH_NAME"
```

The agent will work entirely within this worktree directory.
</worktree_setup>

<single_prompt>

1. Read the complete contents of the prompt file
2. Check if frontend task (see step1c_detect_frontend)
3. Create isolated worktree for this prompt:
   ```bash
   BRANCH="prompt-005-$(date +%s)"
   git worktree add "../.worktrees/$BRANCH" -b "$BRANCH"
   ```
4. Delegate to sub-task with Task tool (subagent_type="general-purpose"):
   - Include worktree path in prompt: "Work in directory: ../.worktrees/$BRANCH"
   - If frontend task, prepend frontend workflow instructions
   - Include: "After completing work, stage and commit all changes in this worktree"
5. Wait for completion
6. Merge worktree back to main:
   ```bash
   git checkout main
   git merge "$BRANCH" --no-ff -m "feat: [prompt description]"
   ```
7. Clean up worktree:
   ```bash
   git worktree remove "../.worktrees/$BRANCH"
   git branch -d "$BRANCH"
   ```
8. Archive prompt to `./prompts/completed/`
9. Return results
</single_prompt>

<parallel_execution>

1. Read all prompt files
2. Analyze each for frontend tasks
3. Create worktree for EACH prompt:
   ```bash
   git worktree add "../.worktrees/prompt-005-$TIMESTAMP" -b "prompt-005-$TIMESTAMP"
   git worktree add "../.worktrees/prompt-006-$TIMESTAMP" -b "prompt-006-$TIMESTAMP"
   git worktree add "../.worktrees/prompt-007-$TIMESTAMP" -b "prompt-007-$TIMESTAMP"
   ```
4. **Spawn all Task tools in a SINGLE MESSAGE** (critical for parallel):
   Each task gets:
   - Its own worktree path
   - Frontend instructions if applicable
   - Instruction to commit within worktree when done
   <example>
   Use Task tool for prompt 005 with worktree "../.worktrees/prompt-005-..."
   Use Task tool for prompt 006 with worktree "../.worktrees/prompt-006-..."
   Use Task tool for prompt 007 with worktree "../.worktrees/prompt-007-..."
   (All in one message with multiple tool calls)
   </example>
5. Wait for ALL to complete
6. Merge all worktrees back sequentially:
   ```bash
   git checkout main
   git merge "prompt-005-$TIMESTAMP" --no-ff -m "feat: [prompt 005 description]"
   git merge "prompt-006-$TIMESTAMP" --no-ff -m "feat: [prompt 006 description]"
   git merge "prompt-007-$TIMESTAMP" --no-ff -m "feat: [prompt 007 description]"
   ```
7. Clean up ALL worktrees:
   ```bash
   git worktree remove "../.worktrees/prompt-005-$TIMESTAMP"
   git worktree remove "../.worktrees/prompt-006-$TIMESTAMP"
   git worktree remove "../.worktrees/prompt-007-$TIMESTAMP"
   git branch -d "prompt-005-$TIMESTAMP" "prompt-006-$TIMESTAMP" "prompt-007-$TIMESTAMP"
   ```
8. Archive all prompts
9. Return consolidated results
</parallel_execution>

<sequential_execution>

For each prompt in order:
1. Read prompt file
2. Check if frontend task
3. Create worktree:
   ```bash
   BRANCH="prompt-XXX-$(date +%s)"
   git worktree add "../.worktrees/$BRANCH" -b "$BRANCH"
   ```
4. Delegate to Task tool with worktree path and frontend instructions if needed
5. Wait for completion
6. Merge back to main:
   ```bash
   git checkout main
   git merge "$BRANCH" --no-ff -m "feat: [prompt description]"
   ```
7. Clean up worktree:
   ```bash
   git worktree remove "../.worktrees/$BRANCH"
   git branch -d "$BRANCH"
   ```
8. Archive prompt
9. **Repeat for next prompt** (each builds on previous merges)

Return consolidated results after all complete.
</sequential_execution>

<merge_conflict_handling>
If merge conflict occurs:
1. Report the conflict to user with details
2. Do NOT auto-resolve
3. Preserve the worktree for manual inspection
4. Provide commands for user to resolve:
   ```bash
   cd ../.worktrees/$BRANCH
   # User resolves conflicts
   git add .
   git commit
   # Then from main repo:
   git merge $BRANCH
   git worktree remove "../.worktrees/$BRANCH"
   ```
</merge_conflict_handling>
</step3_execute>
</process>

<context_strategy>
By delegating to a sub-task in an isolated worktree, each agent works independently without risk of merge conflicts. The main conversation stays lean for orchestration while agents work in parallel safely.
</context_strategy>

<output>
<single_prompt_output>
✓ Executed: ./prompts/005-implement-feature.md
✓ Worktree: prompt-005-1704384000 (created → merged → cleaned up)
✓ Merged to main with commit: feat: implement feature
✓ Archived to: ./prompts/completed/005-implement-feature.md

<results>
[Summary of what the sub-task accomplished]
</results>
</single_prompt_output>

<parallel_output>
✓ Execution strategy: PARALLEL (auto-detected: no dependencies, different file areas)
✓ Created 3 worktrees for isolated execution

Prompts executed:
- ./prompts/005-implement-auth.md → prompt-005-xxx (frontend task: used /frontend-design + Playwright)
- ./prompts/006-implement-api.md → prompt-006-xxx
- ./prompts/007-implement-ui.md → prompt-007-xxx (frontend task: used /frontend-design + Playwright)

✓ All merged to main:
  - feat: implement auth system
  - feat: implement API endpoints
  - feat: implement UI components

✓ All worktrees cleaned up
✓ All archived to ./prompts/completed/

<results>
[Consolidated summary of all sub-task results]
</results>
</parallel_output>

<sequential_output>
✓ Execution strategy: SEQUENTIAL (auto-detected: prompt 006 depends on 005, 007 depends on 006)

Execution order:
1. ./prompts/005-setup-database.md
   → Worktree: prompt-005-xxx
   → Merged: feat: setup database schema
   → Cleaned up ✓

2. ./prompts/006-create-migrations.md
   → Worktree: prompt-006-xxx (started from merged state)
   → Merged: feat: create migrations
   → Cleaned up ✓

3. ./prompts/007-seed-data.md
   → Worktree: prompt-007-xxx (started from merged state)
   → Merged: feat: seed data
   → Cleaned up ✓

✓ All archived to ./prompts/completed/

<results>
[Consolidated summary showing progression through each step]
</results>
</sequential_output>
</output>

<critical_notes>

**Worktree Isolation:**
- EVERY agent runs in its own git worktree - never work in the main directory
- Worktrees are created in `../.worktrees/` relative to the repo root
- Each worktree gets a unique branch: `prompt-[number]-[timestamp]`
- Clean up worktrees after merge - never leave orphaned worktrees

**Execution Strategy:**
- Default is --auto: analyze prompt metadata to determine parallel vs sequential
- For parallel: ALL Task tool calls MUST be in a single message
- For sequential: Wait for each Task to complete AND merge before starting next
- Mixed execution supported: parallel groups followed by sequential dependents

**Frontend Tasks:**
- Automatically detected via `<frontend_tooling>` tag or UI-related keywords
- Frontend prompts get `/frontend-design` skill + Playwright MCP instructions prepended
- Agents should capture before/after screenshots for visual validation

**Merge & Cleanup:**
- Always merge with `--no-ff` to preserve branch history
- If merge conflict: preserve worktree, report to user, provide resolution steps
- Only delete worktree after successful merge
- Archive prompts only after successful completion

**Error Handling:**
- If any prompt fails: stop execution, preserve worktree for debugging
- Report which prompt failed and in which worktree
- Provide instructions for user to inspect and resume
</critical_notes>
