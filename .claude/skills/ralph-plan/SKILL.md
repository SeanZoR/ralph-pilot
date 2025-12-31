---
name: ralph-plan
description: Plan and configure ralph-loop runs. Use this skill when someone wants to run /ralph-loop or needs help structuring an iterative AI task. Helps write effective prompts, set appropriate iteration limits, and apply best practices for autonomous loop execution.
---

# Ralph Loop Planner

This skill helps you plan and configure `/ralph-loop` commands for autonomous, iterative AI tasks.

> **Requires:** [ralph-wiggum plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)

## When to Use

Invoke `/ralph-plan` when you want to:
- Run an autonomous loop task with `/ralph-loop`
- Get help writing an effective loop prompt
- Determine the right `--max-iterations` value
- Structure a task for self-improving iteration

## Planning Process

When helping plan a ralph-loop, follow these steps:

### Step 0: Check Prerequisites

Before planning, verify ralph-wiggum is installed:

1. Ask the user: "Do you have the ralph-wiggum plugin installed? (You should have access to `/ralph-loop`)"

2. If NOT installed, help them install it:
   ```
   /install ralph-wiggum
   ```

   Or they can browse plugins with `/plugins` and search for "ralph-wiggum".

3. After installation, they may need to restart Claude Code for the plugin to load.

Only proceed to planning once ralph-wiggum is confirmed installed.

### Step 1: Understand the Task

Ask the user:
1. **What are you trying to build or accomplish?**
2. **Is this greenfield (new) or modifying existing code?**
3. **Do you have automated verification (tests, linters, type checks)?**

### Step 2: Assess Complexity & Suggest Iterations

Based on the task, recommend `--max-iterations`:

| Task Type | Suggested Iterations |
|-----------|---------------------|
| Simple bug fix / small feature | 5-10 |
| Medium feature with tests | 15-25 |
| Complex feature, multiple files | 30-50 |
| Large greenfield project | 50-100 |
| Exploratory / research task | 10-20 (with escape hatch) |

Always set a limit! Never run unlimited loops.

### Step 3: Structure the Prompt Using Best Practices

Help the user build their prompt with these elements:

#### A. Clear Completion Criteria
```
When complete:
- [ ] Specific deliverable 1
- [ ] Specific deliverable 2
- [ ] All tests passing
- [ ] Output: <promise>COMPLETE</promise>
```

#### B. Incremental Phases (for larger tasks)
```
Phase 1: [Foundation] - description
Phase 2: [Core Feature] - description
Phase 3: [Polish] - description
```

#### C. Self-Correction Loop
```
After each iteration:
1. Run tests/linter/type-check
2. If failures, analyze and fix
3. Commit working changes
4. Continue to next phase
```

#### D. Escape Hatch (always include!)
```
If stuck after [N] iterations:
- Document blockers in BLOCKERS.md
- List attempted solutions
- Suggest alternative approaches
- Output: <promise>STUCK</promise>
```

### Step 4: Generate the Command

Construct the final command:

```bash
/ralph-loop "<constructed_prompt>" --max-iterations <N> --completion-promise "COMPLETE"
```

### Step 5: Pre-flight Checklist

Before running, confirm:
- [ ] Working directory is correct
- [ ] Git is clean (committed or stashed)
- [ ] Tests currently pass (if applicable)
- [ ] Prompt has clear success criteria
- [ ] `--max-iterations` is set
- [ ] Escape hatch is defined in prompt

## Example Output

For a user saying "I want to build a REST API for todos":

```bash
/ralph-loop "Build a REST API for todo items.

Requirements:
- CRUD endpoints: GET/POST/PUT/DELETE /todos
- Input validation (title required, max 200 chars)
- In-memory storage (no database needed)
- Tests with >80% coverage

Process:
1. Set up project structure
2. Implement endpoints one by one with tests
3. Add validation layer
4. Run all tests after each change
5. Fix any failures before proceeding

When complete:
- All endpoints working (verify with curl examples)
- All tests passing
- README with API documentation

If stuck after 15 iterations:
- Document blockers in BLOCKERS.md
- Output: <promise>STUCK</promise>

Output <promise>COMPLETE</promise> when all requirements met." --max-iterations 25 --completion-promise "COMPLETE"
```

## Important Reminders

1. **Ralph works best with clear, verifiable goals** - Tests, linters, and type checkers provide automatic feedback
2. **Iteration limits are safety nets** - Always use `--max-iterations`
3. **Prompts are immutable** - The same prompt runs each iteration; Claude sees progress via file changes
4. **Walk away** - Ralph is designed for autonomous operation; check back later
