# claude

A repository for defining [rulesets](https://code.claude.com/docs/memory), guidelines, [commands](https://code.claude.com/docs/commands), [skills](https://code.claude.com/docs/skills), [hooks](https://code.claude.com/docs/hooks), [MCP servers](https://code.claude.com/docs/mcp), and [settings](https://code.claude.com/docs/settings) for [Claude Code](https://code.claude.com/docs).

## Overview

This repository contains rules and best practices that Claude Code references when working with codebases.

## Structure

- **CLAUDE.md** - Main instruction file for Claude Code
- **.claude/** - Configuration and guidelines
  - `commands/` - Custom slash commands
  - `skills/` - Custom skills with arguments
  - `hooks/` - Hook configuration templates
  - `mcp/` - MCP server configuration templates
  - `settings/` - Settings templates
  - `architecture/` - Common architecture patterns
  - `go/` - Go language style, guidelines, and patterns
  - `rust/` - Rust style, API design, and patterns
  - `typescript/` - TypeScript style and guidelines

## Commands

| Command | Description |
|---------|-------------|
| `/review` | Review code changes in current branch |
| `/explain` | Explain code in a file |
| `/test` | Run tests for the project |
| `/lint` | Run linter for the project |
| `/format` | Format code in the project |
| `/build` | Build the project |
| `/status` | Show git status |

## Skills

| Skill | Description |
|-------|-------------|
| `/refactor <target>` | Refactor specified code |
| `/fix <issue>` | Fix a reported issue |
| `/doc <target>` | Generate documentation |
| `/add <feature>` | Add a new feature |
| `/search <term>` | Search codebase for term |
| `/rename <old> <new>` | Rename across codebase |
| `/debug <issue>` | Debug an issue |
| `/commit-message` | Generate commit message from staged changes |
| `/diagram <target>` | Create ASCII diagram |
| `/migrate <target>` | Migrate code to new version/framework |
| `/security <target>` | Security audit |

## Usage

Apply the contents of this repository to your project to enable Claude Code to generate and review code following consistent coding conventions.
