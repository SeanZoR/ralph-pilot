# ralph-planner

A Claude Code skill that helps you plan and configure `/ralph-loop` runs for autonomous, iterative AI tasks.

> **Built on [ralph-wiggum](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)** - You must install the ralph-wiggum plugin first. This skill helps you use it effectively.

## Prerequisites

Install the ralph-wiggum plugin before using ralph-planner:

```bash
/install ralph-wiggum
```

Or browse available plugins:
```bash
/plugins
```

## What is Ralph?

Ralph is a technique for running Claude Code in an autonomous loop:

```bash
/ralph-loop "Build a REST API" --max-iterations 25 --completion-promise "COMPLETE"
```

Claude works, tries to exit, gets the same prompt again, sees its previous work in the files, and continues improving until done. It's remarkably effective for well-defined tasks.

## What is ralph-planner?

Writing good ralph-loop prompts requires following best practices:
- Clear completion criteria
- Incremental phases
- Self-correction loops
- Escape hatches
- Appropriate iteration limits

**ralph-planner** is a Claude Code skill that guides you through creating effective loop prompts.

## Installation

### Option 1: Personal installation (just for you)

```bash
# Clone or copy the skill to your Claude skills directory
mkdir -p ~/.claude/skills
cp -r .claude/skills/ralph-plan ~/.claude/skills/
```

### Option 2: Project installation (shared with team)

```bash
# Copy to your project's .claude directory
cp -r .claude/skills/ralph-plan /path/to/your/project/.claude/skills/
```

### Option 3: Symlink (for development)

```bash
ln -s /path/to/ralph-planner/.claude/skills/ralph-plan ~/.claude/skills/ralph-plan
```

After installation, restart Claude Code. The skill loads at startup.

## Usage

In Claude Code, invoke the skill:

```
/ralph-plan
```

The skill will guide you through:

1. **Understanding your task** - What you're building, greenfield vs existing, verification available
2. **Suggesting iteration limits** - Based on task complexity
3. **Structuring your prompt** - Applying all best practices
4. **Generating the command** - Ready to copy and run
5. **Pre-flight checklist** - Confirming you're ready

### Example Session

```
You: /ralph-plan

Claude: What are you trying to build or accomplish?

You: A CLI tool that converts markdown to HTML

Claude: Is this a new project or modifying existing code?

You: New project

Claude: Do you have automated verification (tests, linters)?

You: I'll add tests

Claude: Based on this medium-complexity task, I recommend --max-iterations 25.

Here's your structured prompt:
[Generated prompt with all best practices applied]

Pre-flight checklist:
- [ ] Working directory is correct
- [ ] Git is clean
...
```

## Best Practices Reference

The skill includes a `best-practices.md` file covering:

- The 4 pillars of good prompts
- Iteration limit guidelines
- Good vs bad task types
- Ready-to-use templates
- Common pitfalls

## Why Use This?

| Without ralph-planner | With ralph-planner |
|-----------------------|-------------------|
| Forget `--max-iterations` | Always prompted for limits |
| Vague completion criteria | Structured success checklist |
| No escape hatch | Built-in stuck detection |
| Trial and error prompts | Proven templates |
| Loops run too long | Right-sized iterations |

## Credits

- [Ralph technique](https://ghuntley.com/ralph/) by Geoffrey Huntley
- [ralph-wiggum plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum) by Anthropic

## License

MIT
