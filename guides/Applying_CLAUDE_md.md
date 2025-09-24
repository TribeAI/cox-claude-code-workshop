# Applying CLAUDE.md

This file guides Claude’s behavior and persists project/user/enterprise memories.

## Where to put it (scopes)
- Project memory: `./CLAUDE.md` or `./.claude/CLAUDE.md`  
- User memory: `~/.claude/CLAUDE.md`  
- Enterprise policy (IT‑managed): OS‑specific system paths  
See: [Determine memory type](https://anthropic.mintlify.app/en/docs/claude-code/memory#determine-memory-type).

## Initialize / edit
- Bootstrap a project memory: `/init`  
- Open and edit memory files: `/memory`  
References: [Set up project memory](https://anthropic.mintlify.app/en/docs/claude-code/memory#set-up-project-memory) • [Directly edit memories](https://anthropic.mintlify.app/en/docs/claude-code/memory#directly-edit-memories-with-memory).

## Import additional files
Use `@path` lines inside CLAUDE.md to pull in other files:
```
See @README for an overview and @package.json for npm scripts.

# Additional Instructions
- git workflow @docs/git-instructions.md
- @~/.claude/my-project-instructions.md
```
- Imports support relative/absolute paths; nested imports up to depth 5.  
- Imports in code blocks/spans are ignored.  
Details: [CLAUDE.md imports](https://anthropic.mintlify.app/en/docs/claude-code/memory#claude-md-imports).

## Practical guidelines
Keep memories concise, actionable, and versioned; prefer bullet points and group instructions by heading. Review periodically as architecture and conventions change. See: [Best practices](https://anthropic.mintlify.app/en/docs/claude-code/memory#memory-best-practices).

## Handy shortcut
Start a line with `#` to quickly add a memory (select the file to store it): [Quickly add memories](https://anthropic.mintlify.app/en/docs/claude-code/memory#quickly-add-memories-with-the--shortcut).
