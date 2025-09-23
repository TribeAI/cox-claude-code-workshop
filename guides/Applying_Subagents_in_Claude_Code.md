# Applying Subagents in Claude Code

## What they are
Subagents are specialized assistants with their own system prompt, tool permissions, and optional model selection. They can be invoked automatically or explicitly.

## Create and manage subagents
- Interactive creation and editing:
```
/agents
```
Use the UI to create project-level or user-level subagents, set tools, and edit prompts.

- File locations (Markdown with YAML frontmatter):
  - Project scope: `.claude/agents/`
  - User scope: `~/.claude/agents/`
  Project agents take precedence on name conflicts.

- Minimal file format example:
```md
---
name: code-reviewer
description: Expert code review specialist for JS/TS and React
tools: Read, Grep, Glob, Bash
model: inherit
---
You are a senior code reviewer. Provide actionable feedback with examples.
```

## Use subagents
- Delegation: Claude may proactively delegate based on your request and the subagent description.
- Explicit invocation (natural language):
```
Use the code-reviewer subagent to review the last commit.
```
