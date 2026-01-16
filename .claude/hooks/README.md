# Hooks

[Hooks](https://code.claude.com/docs/hooks) allow you to run custom commands at various points during Claude Code execution.

## Configuration

Add hooks to `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "echo 'Running Bash command...'"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "npm run lint --fix $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

## Hook Types

| Hook | Trigger |
|------|---------|
| `PreToolUse` | Before a tool is executed |
| `PostToolUse` | After a tool completes |
| `Notification` | When Claude sends a notification |

## Examples

### Auto-format on file write

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "prettier --write $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### Run tests after code changes

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npm test"
      }
    ]
  }
}
```

### Lint before commit

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit)",
        "command": "npm run lint"
      }
    ]
  }
}
```
