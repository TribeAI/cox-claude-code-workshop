# Applying MCP in Claude Code

## What MCP is (and why it matters)
Claude Code can connect to external tools and data via the **Model Context Protocol (MCP)**. Once connected, Claude can call those tools and their exposed prompts directly from your session. See the official guide: [Connect Claude Code to tools via MCP](https://docs.anthropic.com/en/docs/claude-code/mcp)

## Add a server (stdio + remote)
**Local stdio (common for OSS servers):**
```bash
# Basic syntax (from docs)
claude mcp add <name> <command> [args...]

# Example (Airtable, from docs)
claude mcp add airtable --env AIRTABLE_API_KEY=YOUR_KEY -- npx -y airtable-mcp-server
```
> The `--` separates Claude flags from the server command. Everything after `--` is the actual command to launch the MCP server. See: [Adding a local stdio server](https://docs.anthropic.com/en/docs/claude-code/mcp#option-1-add-a-local-stdio-server).

**Remote servers (HTTP / SSE):**
```bash
# HTTP example (Sentry)
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# SSE example (Asana)
claude mcp add --transport sse asana https://mcp.asana.com/sse
```
Popular examples are listed under [MCP servers](https://docs.anthropic.com/en/docs/claude-code/mcp#popular-mcp-servers).

## Manage servers and OAuth
Use the interactive slash command:
```
/mcp
```
View configured servers, connection status, exposed tools/prompts, and handle OAuth. Reference: [Slash commands → Managing MCP connections](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#managing-mcp-connections).

## Use MCP prompts as slash commands
When servers expose prompts, they appear automatically as slash commands:
```
/mcp__<server-name>__<prompt-name> [arguments]
```
Examples:
```
/mcp__github__list_prs
/mcp__jira__create_issue "Bug title" high
```
See: [MCP slash commands](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#mcp-slash-commands).

## Permissions (no wildcards)
When approving MCP tools:
- `mcp__github` (all tools from that server)
- `mcp__github__get_issue` (one tool)
- Not supported: `mcp__github__*`  
Details: [Permissions & wildcards](https://anthropic.mintlify.app/en/docs/claude-code/slash-commands#mcp-permissions-and-wildcards).

## Practical workflow
1. Add the server (stdio/HTTP/SSE).  
2. Run `/mcp` → confirm **Connected** and browse tools/prompts.  
3. Try a server prompt via `/mcp__server__prompt`.  
4. Use in a task prompt: “Use the **mcp__github** tools to open a PR after tests pass.”

## Troubleshooting tips
- If a server fails to connect, re-run `/mcp`, check auth, and confirm the server process is running (for stdio).  
- If slash commands don’t appear, reconnect and verify the server exposes prompts.  
- Consider configuration scope if sharing set‑ups (local/project/user): [MCP installation scopes](https://docs.anthropic.com/en/docs/claude-code/mcp#mcp-installation-scopes).
