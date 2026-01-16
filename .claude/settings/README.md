# Settings

[Settings](https://code.claude.com/docs/settings) configure Claude Code behavior for your project.

## File Locations

| File | Scope | Git |
|------|-------|-----|
| `.claude/settings.json` | Project (shared) | Commit |
| `.claude/settings.local.json` | Project (personal) | Ignore |
| `~/.claude/settings.json` | User global | N/A |

## Example Settings

### `.claude/settings.json` (Team shared)

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(go test *)",
      "Bash(cargo test *)",
      "Read",
      "Write",
      "Edit"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "npm run lint --fix $CLAUDE_FILE_PATH"
      }
    ]
  }
}
```

### `.claude/settings.local.json` (Personal)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-personal-token"
      }
    }
  }
}
```

## Common Permissions

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(npx *)",
      "Bash(yarn *)",
      "Bash(pnpm *)",
      "Bash(go *)",
      "Bash(cargo *)",
      "Bash(make *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Read",
      "Write",
      "Edit",
      "Glob",
      "Grep"
    ]
  }
}
```
