# ralph-pilot

A Claude Code skill that helps you plan and configure `/ralph-loop` runs for autonomous, iterative AI tasks.

> **Built on [ralph-wiggum](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)** - You must install the ralph-wiggum plugin first. This skill helps you use it effectively.

## Prerequisites

Install the ralph-wiggum plugin before using ralph-pilot:

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
/ralph-loop "Build a REST API" --max-iterations 25 --max-time 90m --completion-promise "COMPLETE"
```

Claude works, tries to exit, gets the same prompt again, sees its previous work in the files, and continues improving until done. It's remarkably effective for well-defined tasks.

## What is ralph-pilot?

Writing good ralph-loop prompts requires following best practices:
- Clear completion criteria
- Incremental phases
- Self-correction loops
- Escape hatches
- Appropriate iteration AND time limits

**ralph-pilot** is a Claude Code skill that **interactively guides you** through creating effective loop prompts.

### Key Features

- **Interactive Step-by-Step Guidance** - Walks you through 6 clear steps with visual progress indicators
- **Dual Limit System** - Configure both iteration limits AND time limits for maximum safety
- **Smart Recommendations** - Suggests appropriate limits based on your task complexity
- **Pre-flight Checklist** - Confirms everything is ready before launching

## Installation

### Option 1: Personal installation (just for you)

```bash
# Clone or copy the skill to your Claude skills directory
mkdir -p ~/.claude/skills
cp -r .claude/skills/ralph-pilot ~/.claude/skills/
```

### Option 2: Project installation (shared with team)

```bash
# Copy to your project's .claude directory
cp -r .claude/skills/ralph-pilot /path/to/your/project/.claude/skills/
```

### Option 3: Symlink (for development)

```bash
ln -s /path/to/ralph-pilot/.claude/skills/ralph-pilot ~/.claude/skills/ralph-pilot
```

After installation, restart Claude Code. The skill loads at startup.

## Usage

In Claude Code, invoke the skill:

```
/ralph-pilot
```

The skill interactively guides you through **6 steps**:

| Step | What Happens |
|------|--------------|
| 1. Prerequisites | Confirms ralph-wiggum plugin is installed |
| 2. Task Understanding | Gathers details about what you're building |
| 3. Setting Limits | Configures iterations AND time limits |
| 4. Prompt Building | Structures your prompt with best practices |
| 5. Command Generation | Creates your ready-to-run command |
| 6. Pre-flight Checklist | Final confirmation before launch |

Each step shows clear progress indicators and lets you make choices interactively.

### Example Session

```
You: /ralph-pilot

╔══════════════════════════════════════════════════════════════╗
║  🚀 RALPH LOOP PLANNER                                       ║
║  Let's configure your autonomous AI task                     ║
╠══════════════════════════════════════════════════════════════╣
║  Step 1 of 6: Prerequisites Check                            ║
╚══════════════════════════════════════════════════════════════╝

Claude: Do you have the ralph-wiggum plugin installed?
> You: Yes, I have /ralph-loop available

╔══════════════════════════════════════════════════════════════╗
║  Step 2 of 6: Understanding Your Task                        ║
╚══════════════════════════════════════════════════════════════╝

Claude: What are you trying to build?
> You: A CLI tool that converts markdown to HTML

Claude: What type of project is this?
> You select: Greenfield (new project from scratch)

╔══════════════════════════════════════════════════════════════╗
║  Step 3 of 6: Setting Safety Limits                          ║
╠══════════════════════════════════════════════════════════════╣
║  📊 Recommended Iterations: 25                               ║
║  ⏱️  Recommended Time:       90 minutes                       ║
╚══════════════════════════════════════════════════════════════╝

> You select: Use the recommended limits

[... continues through remaining steps ...]

╔══════════════════════════════════════════════════════════════╗
║  🚀 READY TO LAUNCH                                          ║
╠══════════════════════════════════════════════════════════════╣
║  📊 Will run up to 25 iterations                             ║
║  ⏱️  Will stop after 90 minutes (whichever comes first)      ║
╚══════════════════════════════════════════════════════════════╝
```

## Best Practices Reference

The skill includes a `best-practices.md` file covering:

- The 4 pillars of good prompts
- Iteration limit guidelines
- Good vs bad task types
- Ready-to-use templates
- Common pitfalls

## Why Use This?

| Without ralph-pilot | With ralph-pilot |
|-----------------------|-------------------|
| Forget `--max-iterations` | Always prompted for BOTH iteration AND time limits |
| Vague completion criteria | Structured success checklist |
| No escape hatch | Built-in stuck detection |
| Trial and error prompts | Proven templates |
| Loops run too long | Right-sized iterations with time caps |
| Confusing configuration | Clear 6-step interactive wizard |

## Credits

- [Ralph technique](https://ghuntley.com/ralph/) by Geoffrey Huntley
- [ralph-wiggum plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum) by Anthropic

## License

MIT
