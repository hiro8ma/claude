# Commands & Skills

Custom slash commands for [Claude Code](https://code.claude.com/docs).

- [Commands](https://code.claude.com/docs/commands) - Simple commands without arguments
- [Skills](https://code.claude.com/docs/skills) - Commands that accept arguments

## Usage

Place `.md` files in `.claude/commands/` directory. The filename becomes the command name.

```
.claude/commands/review.md  →  /review
.claude/commands/fix.md     →  /fix <issue>
```

## Commands (No Arguments)

| Command | Description |
|---------|-------------|
| `/review` | Review code changes in current branch |
| `/explain` | Explain code in a file |
| `/test` | Run tests for the project |

## Skills (With Arguments)

Use `$ARGUMENTS` placeholder to accept user input.

| Skill | Description |
|-------|-------------|
| `/refactor <target>` | Refactor specified code |
| `/fix <issue>` | Fix a reported issue |
| `/doc <target>` | Generate documentation |

## Creating Commands

### Simple Command

```markdown
<!-- review.md -->
Review the code changes in the current branch.

1. Run `git diff` to see changes
2. Analyze for bugs, security issues, performance
3. Provide actionable feedback
```

### Skill with Arguments

```markdown
<!-- fix.md -->
Fix the issue: $ARGUMENTS

1. Understand the reported issue
2. Locate the relevant code
3. Implement a fix
4. Verify and run tests
```
