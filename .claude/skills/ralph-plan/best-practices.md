# Ralph Loop Best Practices Reference

## The Four Pillars of Good Ralph Prompts

### 1. Clear Completion Criteria

Your prompt must define exactly what "done" looks like.

**Bad:**
```
Build a todo API and make it good.
```

**Good:**
```
Build a REST API for todos.

When complete:
- All CRUD endpoints working
- Input validation in place
- Tests passing (coverage > 80%)
- README with API docs
- Output: <promise>COMPLETE</promise>
```

### 2. Incremental Goals

Break large tasks into phases. Claude can verify each phase before moving on.

**Bad:**
```
Create a complete e-commerce platform.
```

**Good:**
```
Phase 1: User authentication (JWT, tests)
Phase 2: Product catalog (list/search, tests)
Phase 3: Shopping cart (add/remove, tests)

Complete each phase fully before moving to next.
Output <promise>COMPLETE</promise> when all phases done.
```

### 3. Self-Correction Loops

Build in automatic verification and fixing.

**Bad:**
```
Write code for feature X.
```

**Good:**
```
Implement feature X following TDD:
1. Write failing tests
2. Implement feature
3. Run tests
4. If any fail, debug and fix
5. Refactor if needed
6. Repeat until all green
7. Output: <promise>COMPLETE</promise>
```

### 4. Escape Hatches

Always define what to do when stuck.

```
If stuck after 15 iterations:
- Create BLOCKERS.md with:
  - What's blocking progress
  - What was attempted
  - Suggested alternatives
- Output: <promise>STUCK</promise>
```

---

## Iteration Limit Guidelines

| Task Complexity | Examples | Suggested Limit |
|-----------------|----------|-----------------|
| **Simple** | Fix typo, add config, small refactor | 5-10 |
| **Medium** | Single feature with tests, bug fix requiring investigation | 15-25 |
| **Complex** | Multi-file feature, API with validation | 30-50 |
| **Large** | New project from scratch, major refactor | 50-100 |

**Rule of thumb:** If unsure, start with 20 iterations. You can always run again.

---

## Good vs Bad Task Types

### Good for Ralph

- **Well-defined tasks** with clear success criteria
- **Tasks with automatic verification** (tests, linters, type checkers)
- **Greenfield projects** where you can walk away
- **Iteration-friendly work** (getting tests to pass, refactoring)

### Not Good for Ralph

- **Tasks requiring human judgment** or design decisions
- **One-shot operations** (deploying, publishing)
- **Unclear success criteria** ("make it better")
- **Production debugging** (needs human oversight)

---

## Prompt Templates

### Template 1: Feature with Tests

```
Implement [FEATURE NAME].

Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Process:
1. Write failing tests for each requirement
2. Implement to make tests pass
3. Run full test suite after each change
4. Fix any regressions before proceeding

When complete:
- All requirements implemented
- All tests passing
- Code is clean and documented

If stuck after [N] iterations:
- Document blockers in BLOCKERS.md
- Output: <promise>STUCK</promise>

Output <promise>COMPLETE</promise> when done.
```

### Template 2: Bug Fix

```
Fix the bug: [DESCRIPTION]

Steps:
1. Write a failing test that reproduces the bug
2. Investigate root cause
3. Implement fix
4. Verify test passes
5. Run full test suite
6. Check for regressions

When complete:
- Bug is fixed
- Regression test added
- All tests passing

Output <promise>COMPLETE</promise> when fixed.
```

### Template 3: Refactoring

```
Refactor [COMPONENT/MODULE].

Goals:
- [Improvement 1]
- [Improvement 2]

Constraints:
- All existing tests must pass
- No behavior changes
- Keep backwards compatibility

Process:
1. Run tests (must pass before starting)
2. Make incremental changes
3. Run tests after each change
4. Commit when green
5. Repeat until goals met

Output <promise>COMPLETE</promise> when all goals achieved.
```

---

## Common Pitfalls

### 1. Vague Success Criteria
**Problem:** "Make the code better"
**Fix:** Define measurable outcomes

### 2. No Iteration Limit
**Problem:** Loop runs forever
**Fix:** Always use `--max-iterations`

### 3. No Escape Hatch
**Problem:** Claude spins on impossible tasks
**Fix:** Define what to do when stuck

### 4. Too Ambitious
**Problem:** Trying to build too much in one loop
**Fix:** Break into smaller, focused loops

### 5. No Automatic Verification
**Problem:** No way to know if changes work
**Fix:** Add tests, linting, or type checking
