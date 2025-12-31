---
name: ralph-pilot
description: Plan and configure ralph-loop runs. Use this skill when someone wants to run /ralph-loop or needs help structuring an iterative AI task. Helps write effective prompts, set appropriate iteration limits, and apply best practices for autonomous loop execution.
---

# Ralph Loop Planner

This skill helps you plan and configure `/ralph-loop` commands for autonomous, iterative AI tasks.

> **Requires:** [ralph-wiggum plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)

## When to Use

Invoke `/ralph-pilot` when you want to:
- Run an autonomous loop task with `/ralph-loop`
- Get help writing an effective loop prompt
- Determine the right `--max-iterations` and `--max-time` values
- Structure a task for self-improving iteration

---

## 🎯 INTERACTIVE MODE INSTRUCTIONS

**IMPORTANT:** When this skill is invoked, you MUST guide the user interactively through each step. Use the **AskUserQuestion** tool to present clear choices at each stage. DO NOT dump all information at once.

### How to Guide the User

1. **One step at a time** - Ask one question, wait for answer, then proceed
2. **Show progress** - Tell the user which step they're on (e.g., "Step 2 of 5")
3. **Summarize choices** - After each answer, confirm what was selected
4. **Visual checkpoints** - Show a running summary of their configuration

---

## Planning Process

When helping plan a ralph-loop, follow these steps interactively:

### Step 0: Check Prerequisites (1 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  🚀 RALPH LOOP PLANNER                                       ║
║  Let's configure your autonomous AI task                     ║
╠══════════════════════════════════════════════════════════════╣
║  Step 1 of 6: Prerequisites Check                            ║
╚══════════════════════════════════════════════════════════════╝
```

**Use AskUserQuestion** with these options:
- Question: "Do you have the ralph-wiggum plugin installed?"
- Options:
  1. "Yes, I have /ralph-loop available"
  2. "No, I need to install it"
  3. "Not sure, help me check"

**If NOT installed**, help them install it:
```
/install ralph-wiggum
```
Or browse plugins with `/plugins` and search for "ralph-wiggum".

After installation, they may need to restart Claude Code.

Only proceed once ralph-wiggum is confirmed installed.

### Step 1: Understand the Task (2 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  Step 2 of 6: Understanding Your Task                        ║
╚══════════════════════════════════════════════════════════════╝
```

**Ask in natural conversation:**
1. **What are you trying to build or accomplish?** (let them describe freely)

**Then use AskUserQuestion:**
- Question: "What type of project is this?"
- Options:
  1. "Greenfield (new project from scratch)"
  2. "Modifying existing code"
  3. "Bug fix in existing codebase"
  4. "Refactoring/cleanup"

**Then use AskUserQuestion:**
- Question: "Do you have automated verification?"
- Options:
  1. "Yes - tests, linting, and type checking"
  2. "Partial - some tests or linting"
  3. "No - I'll add verification as part of this task"
  4. "No - this task doesn't need it"

### Step 2: Set Limits - Iterations AND Time (3 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  Step 3 of 6: Setting Safety Limits                          ║
╠══════════════════════════════════════════════════════════════╣
║  Ralph uses TWO types of limits to prevent runaway loops:    ║
║                                                              ║
║  📊 ITERATIONS - How many times Claude restarts              ║
║  ⏱️  TIME LIMIT - Maximum wall-clock time                     ║
║                                                              ║
║  Whichever limit is hit first will stop the loop.            ║
╚══════════════════════════════════════════════════════════════╝
```

**Based on the task, SHOW this recommendation table:**

```
┌─────────────────────────────┬────────────────┬─────────────┐
│ Task Type                   │ Iterations     │ Time Limit  │
├─────────────────────────────┼────────────────┼─────────────┤
│ Simple bug fix / tweak      │ 5-10           │ 15-30 min   │
│ Medium feature with tests   │ 15-25          │ 45-90 min   │
│ Complex multi-file feature  │ 30-50          │ 2-3 hours   │
│ Large greenfield project    │ 50-100         │ 4-8 hours   │
│ Exploratory / research      │ 10-20          │ 30-60 min   │
└─────────────────────────────┴────────────────┴─────────────┘
```

**Use AskUserQuestion:**
- Question: "Based on your task, I recommend X iterations and Y time. How would you like to proceed?"
- Options:
  1. "Use the recommended limits"
  2. "I want to set custom limits"
  3. "Use only iteration limit (no time limit)"
  4. "Use only time limit (no iteration limit)"

**IMPORTANT:** Always set at least ONE limit! Never run unlimited loops.

**After selection, display the chosen limits prominently:**
```
╔══════════════════════════════════════════════════════════════╗
║  ✅ LIMITS CONFIGURED                                        ║
╠══════════════════════════════════════════════════════════════╣
║  📊 Max Iterations: [NUMBER]                                 ║
║  ⏱️  Max Time:       [DURATION]                               ║
╚══════════════════════════════════════════════════════════════╝
```

### Step 3: Structure the Prompt (4 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  Step 4 of 6: Building Your Prompt                           ║
╠══════════════════════════════════════════════════════════════╣
║  A good ralph prompt has 4 key elements:                     ║
║                                                              ║
║  ✅ Completion Criteria  - How Claude knows it's done        ║
║  📋 Phases (optional)    - Breaking work into stages         ║
║  🔄 Self-Correction      - How to fix issues                 ║
║  🚪 Escape Hatch         - What to do when stuck             ║
╚══════════════════════════════════════════════════════════════╝
```

**Use AskUserQuestion:**
- Question: "How should we structure your task?"
- Options:
  1. "Help me define each element step by step"
  2. "Show me a template I can customize"
  3. "Generate a prompt based on my task description"

**For each element, guide interactively:**

#### A. Clear Completion Criteria
**Ask:** "What specific deliverables should be complete? List 2-4 concrete outcomes."
```
When complete:
- [ ] Specific deliverable 1
- [ ] Specific deliverable 2
- [ ] All tests passing
- [ ] Output: <promise>COMPLETE</promise>
```

#### B. Incremental Phases (for larger tasks)
**Use AskUserQuestion:**
- Question: "Does this task need phases?"
- Options:
  1. "Yes - break it into phases for me"
  2. "No - it's simple enough to do in one go"

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

#### D. Escape Hatch (ALWAYS include!)
```
If stuck after [N] iterations OR [TIME]:
- Document blockers in BLOCKERS.md
- List attempted solutions
- Suggest alternative approaches
- Output: <promise>STUCK</promise>
```

### Step 4: Generate the Command (5 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  Step 5 of 6: Your Ralph Command                             ║
╚══════════════════════════════════════════════════════════════╝
```

**Construct and display the final command:**

```bash
/ralph-loop "<constructed_prompt>" \
  --max-iterations <N> \
  --max-time <DURATION> \
  --completion-promise "COMPLETE"
```

**Display a clear summary box:**
```
╔══════════════════════════════════════════════════════════════╗
║  📋 CONFIGURATION SUMMARY                                    ║
╠══════════════════════════════════════════════════════════════╣
║  Task:           [Brief description]                         ║
║  Type:           [Greenfield/Modification/Bug fix/Refactor]  ║
║  Verification:   [Yes/Partial/No]                            ║
╠──────────────────────────────────────────────────────────────╣
║  📊 Max Iterations: [NUMBER]                                 ║
║  ⏱️  Max Time:       [DURATION]                               ║
║  🎯 Promise:        COMPLETE                                 ║
╚══════════════════════════════════════════════════════════════╝
```

**Note on time format:** Use formats like `30m`, `1h`, `2h30m`, `4h`

### Step 5: Pre-flight Checklist (6 of 6)

**Show the user:**
```
╔══════════════════════════════════════════════════════════════╗
║  Step 6 of 6: Pre-flight Checklist                           ║
╠══════════════════════════════════════════════════════════════╣
║  Let's make sure everything is ready before launch!          ║
╚══════════════════════════════════════════════════════════════╝
```

**Use AskUserQuestion with multiSelect:true:**
- Question: "Please confirm these pre-flight checks:"
- Options (all should be confirmed):
  1. "Working directory is correct"
  2. "Git is clean (committed or stashed)"
  3. "Tests currently pass (if applicable)"
  4. "I understand the limits I've set"

**After confirmation, show:**
```
╔══════════════════════════════════════════════════════════════╗
║  🚀 READY TO LAUNCH                                          ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║  Your ralph-loop is configured and ready!                    ║
║                                                              ║
║  📊 Will run up to [N] iterations                            ║
║  ⏱️  Will stop after [TIME] (whichever comes first)          ║
║                                                              ║
║  Copy the command above and run it.                          ║
║  You can walk away - Ralph will work autonomously!           ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

**Use AskUserQuestion:**
- Question: "What would you like to do?"
- Options:
  1. "Run the command now"
  2. "Copy to clipboard and run later"
  3. "Modify the configuration"
  4. "Start over with a different task"

## Example Session

For a user saying "I want to build a REST API for todos":

```
╔══════════════════════════════════════════════════════════════╗
║  🚀 RALPH LOOP PLANNER                                       ║
║  Let's configure your autonomous AI task                     ║
╠══════════════════════════════════════════════════════════════╣
║  Step 1 of 6: Prerequisites Check                            ║
╚══════════════════════════════════════════════════════════════╝
```

> User confirms ralph-wiggum is installed

```
╔══════════════════════════════════════════════════════════════╗
║  Step 2 of 6: Understanding Your Task                        ║
╚══════════════════════════════════════════════════════════════╝
```

> User: "Build a REST API for todo items"
> User selects: "Greenfield (new project from scratch)"
> User selects: "No - I'll add verification as part of this task"

```
╔══════════════════════════════════════════════════════════════╗
║  Step 3 of 6: Setting Safety Limits                          ║
╠══════════════════════════════════════════════════════════════╣
║  Based on your task (Medium greenfield with tests):          ║
║                                                              ║
║  📊 Recommended Iterations: 25                               ║
║  ⏱️  Recommended Time:       90 minutes                       ║
╚══════════════════════════════════════════════════════════════╝
```

> User selects: "Use the recommended limits"

**Final generated command:**

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

If stuck after 15 iterations OR 60 minutes:
- Document blockers in BLOCKERS.md
- Output: <promise>STUCK</promise>

Output <promise>COMPLETE</promise> when all requirements met." \
  --max-iterations 25 \
  --max-time 90m \
  --completion-promise "COMPLETE"
```

```
╔══════════════════════════════════════════════════════════════╗
║  📋 CONFIGURATION SUMMARY                                    ║
╠══════════════════════════════════════════════════════════════╣
║  Task:           REST API for todo items                     ║
║  Type:           Greenfield                                  ║
║  Verification:   Will add tests                              ║
╠──────────────────────────────────────────────────────────────╣
║  📊 Max Iterations: 25                                       ║
║  ⏱️  Max Time:       90 minutes                               ║
║  🎯 Promise:        COMPLETE                                 ║
╚══════════════════════════════════════════════════════════════╝
```

## Important Reminders

1. **Ralph works best with clear, verifiable goals** - Tests, linters, and type checkers provide automatic feedback
2. **Set both limits when possible** - Use `--max-iterations` AND `--max-time` for best safety
3. **Prompts are immutable** - The same prompt runs each iteration; Claude sees progress via file changes
4. **Walk away** - Ralph is designed for autonomous operation; check back later

## Time vs Iteration Limits

| Situation | Best Limit Type |
|-----------|-----------------|
| Time-sensitive task | Use `--max-time` primarily |
| Complex task with unknown duration | Use both limits |
| Simple, predictable task | `--max-iterations` is sufficient |
| Running overnight | Use `--max-time` as primary safety |
