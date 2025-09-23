# Applying MCP in Claude Code

## What it is
Claude Code connects to external tools and data via the Model Context Protocol (MCP). Once connected, tools become available to Claude and often surface as slash commands.

## Fast setup
Add an MCP server to Claude Code (local stdio server example). The `--` separates Claude flags from the server command:
```bash
claude mcp add <name> <command> [args...]

# Example
claude mcp add airtable --env AIRTABLE_API_KEY=YOUR_KEY -- npx -y airtable-mcp-server
```

## Manage and use MCP
- Use the interactive command to manage servers (list, status, auth, tokens, tools/prompts):
```
/mcp
```
- MCP prompts may appear as slash commands using the pattern:
```
/mcp__<server-name>__<prompt-name> [arguments]
```
Examples:
```
/mcp__github__list_prs
/mcp__jira__create_issue "Bug title" high
```

## Permissions guidance
Approve tools per server by name (for example, mcp__github or mcp__github__get_issue). Wildcards like mcp__github__* are not supported.
