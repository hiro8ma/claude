# Skills

[Skills](https://code.claude.com/docs/skills) are custom slash commands that accept arguments.

## Usage

Place `.md` files in `.claude/skills/` directory. Use `$ARGUMENTS` placeholder for user input.

```
.claude/skills/fix.md  →  /fix <issue>
```

## Available Skills

| Skill | Description |
|-------|-------------|
| `/refactor <target>` | Refactor specified code |
| `/fix <issue>` | Fix a reported issue |
| `/doc <target>` | Generate documentation |

## Creating Skills

```markdown
<!-- fix.md -->
Fix the issue: $ARGUMENTS

1. Understand the reported issue
2. Locate the relevant code
3. Implement a fix
4. Verify and run tests
```
