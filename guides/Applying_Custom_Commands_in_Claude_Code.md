# Applying Custom Commands in Claude Code

Custom slash commands are Markdown files that define reusable prompts and optional automation. 

## Create commands
**Project commands** (checked into the repo):  
```bash
mkdir -p .claude/commands
echo "Analyze this code for performance issues and suggest optimizations:" > .claude/commands/optimize.md
```
Run with: `/optimize`  
**Personal commands** (available everywhere):  
```bash
mkdir -p ~/.claude/commands
echo "Review this code for security vulnerabilities:" > ~/.claude/commands/security-review.md
```
Run with: `/security-review`  
See: [Project commands](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#project-commands).

## Arguments
Syntax: `/<command-name> [arguments]` (name = filename without `.md`).  
Capture all args with `$ARGUMENTS`; use positional args with `$1`, `$2`, etc.  
Examples:
```bash
# Definition
echo 'Fix issue #$ARGUMENTS following our coding standards' > .claude/commands/fix-issue.md
# Usage
/fix-issue 123 high-priority
```
```bash
# Definition
echo 'Review PR #$1 with priority $2 and assign to $3' > .claude/commands/review-pr.md
# Usage
/review-pr 456 high alice
```
Reference: [Arguments](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#arguments).

## Frontmatter and features
Frontmatter keys include `allowed-tools`, `argument-hint`, `description`, `model`.  
```md
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
argument-hint: [message]
description: Create a git commit
model: claude-3-5-haiku-20241022
---
Create a git commit with message: $ARGUMENTS
```
See: [Frontmatter](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#frontmatter).

### Bash execution and file references
Execute allowed Bash with lines starting `!`; reference files with `@path`.  
```md
---
allowed-tools: Bash(git status:*), Bash(git diff:*)
description: Summarize repo changes
---
- Status: !`git status`
- Diff: !`git diff HEAD`

Review the implementation in @src/utils/helpers.js
```
Docs: [Bash execution](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#bash-command-execution) • [File references](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#file-references).

## Tip: MCP slash commands
When MCP servers expose prompts, they appear as:  
```
/mcp__<server-name>__<prompt-name> [arguments]
```
See: [MCP slash commands](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#mcp-slash-commands).
