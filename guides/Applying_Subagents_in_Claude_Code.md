# Applying Subagents in Claude Code

## What subagents are
Subagents are specialized assistants with their own system prompt, tools, and optional model. Claude can delegate automatically based on your request and the agent’s description, or you can invoke them explicitly.

## Create subagents (recommended: interactive)
Open the subagents interface:
```
/agents
```
Create a project-level or user-level subagent, set allowed tools, and edit the prompt. See: [Using the /agents command](https://docs.anthropic.com/en/docs/claude-code/sub-agents#using-the-agents-command-recommended).

## Create subagents (files)
Locations (Markdown with YAML frontmatter):  
- Project scope: `.claude/agents/`  
- User scope: `~/.claude/agents/`  
Reference: [File locations](https://docs.anthropic.com/en/docs/claude-code/sub-agents#file-locations).

Minimal example:
```md
---
name: code-reviewer
description: Expert code review specialist. Proactively reviews code for quality, security, and maintainability.
tools: Read, Grep, Glob, Bash
model: inherit
---
You are a senior code reviewer ensuring high standards of code quality and security.
```
Format details: [Subagent file format](https://docs.anthropic.com/en/docs/claude-code/sub-agents#file-format).

## Using subagents
- Automatic delegation is driven by your task phrasing and the agent’s `description`. You can encourage proactive use (e.g., “MUST BE USED”).  
- Explicit invocation (natural language): “Use the code-reviewer subagent to check my recent changes.”  
See: [Explicit invocation](https://docs.anthropic.com/en/docs/claude-code/sub-agents#explicit-invocation).

## Practical patterns
- Test‑runner agent that runs tests and fixes failing cases.  
- Debugger agent that isolates a failing line and proposes a minimal fix.  
- Data‑scientist agent configured with data tools (inherits MCP tools if `tools` omitted).  
Examples & guidance: [Example subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents#example-subagents).

## Tips
- Prefer project agents for repo‑specific behavior; they take precedence on name conflicts.  
- Keep prompts short and action‑oriented for reliable auto‑delegation.  
- Only grant powerful tools (e.g., `Edit`, `Bash`) to agents that truly need them.
