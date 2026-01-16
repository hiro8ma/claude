# Commands

[Commands](https://code.claude.com/docs/commands) are custom slash commands for Claude Code.

## Usage

Place `.md` files in `.claude/commands/` directory. The filename becomes the command name.

```
.claude/commands/review.md  →  /review
```

## Available Commands

| Command | Description |
|---------|-------------|
| `/review` | Review code changes in current branch |
| `/explain` | Explain code in a file |
| `/test` | Run tests for the project |

## Creating Commands

```markdown
<!-- review.md -->
Review the code changes in the current branch.

1. Run `git diff` to see changes
2. Analyze for bugs, security issues, performance
3. Provide actionable feedback
```
