---
name: commit-message
description: Generate a clear commit message from staged changes
---

Generate a commit message for the current staged changes.

1. Run `git diff --staged` to see changes
2. Analyze the changes and create a commit message:
   - Summary under 50 characters
   - Blank line
   - Detailed description of what changed and why
   - List affected components
3. Follow best practices:
   - Use present tense ("Add feature" not "Added feature")
   - Explain what and why, not how
