# Applying Custom Commands in Claude Code

## What they are
Custom slash commands are Markdown files defining reusable prompts. They can accept arguments, declare frontmatter options, and restrict tool access.

## Create commands
- Project commands (shared with the repo):
```bash
mkdir -p .claude/commands
echo "Analyze this code for performance issues and suggest optimizations:" > .claude/commands/optimize.md
```
Run with:
```
/optimize
```

- Personal commands (available in any repo):
```bash
mkdir -p ~/.claude/commands
echo "Review this code for security vulnerabilities:" > ~/.claude/commands/security-review.md
```
Run with:
```
/security-review
```

## Arguments and features
- Syntax: `/command-name [arguments]` (name = filename without `.md`).
- Collect all arguments with `$ARGUMENTS`; use positional args with `$1`, `$2`, etc.

Examples:
```bash
# Definition
echo 'Fix issue #$ARGUMENTS' > .claude/commands/fix-issue.md
# Usage
/fix-issue 123 high
```
```bash
# Definition
echo 'Review PR #$1 with priority $2' > .claude/commands/review-pr.md
# Usage
/review-pr 456 high
```

### Frontmatter options (examples)
`allowed-tools`, `argument-hint`, `description`, `model`.
```md
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
argument-hint: [message]
description: Create a git commit
model: claude-3-5-haiku-20241022
---
Create a git commit with message: $ARGUMENTS
```

### Run Bash or reference files within a command
Use backticked `!` lines to execute allowed Bash, and `@path` to bring files into context:
```md
---
allowed-tools: Bash(git status:*), Bash(git diff:*)
description: Summarize repo changes
---
- Status: !`git status`
- Diff: !`git diff HEAD`

Review the implementation in @src/utils/helpers.js
```

## MCP slash commands
When an MCP server is connected, its prompts appear as `/mcp__server__prompt`.
