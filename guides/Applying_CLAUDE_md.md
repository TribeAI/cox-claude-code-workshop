# Applying CLAUDE.md

## What it is
CLAUDE.md is a memory file Claude loads to guide behavior. You can keep versions at enterprise, project, and user scopes; Claude reads them hierarchically.

## Where to put it
- Project (team-shared): `./CLAUDE.md` or `./.claude/CLAUDE.md`
- User (personal defaults): `~/.claude/CLAUDE.md`

## Initialize and edit
- Bootstrap a memory file:
```
/init
```
- Open and edit memory files:
```
/memory
```

## Compose memories with imports
Inside CLAUDE.md, reference other files using `@path` lines. Imports can be relative or absolute (nested imports are supported to a limited depth).
```
See @README and @package.json for project info.
# Additional Instructions
- git workflow @docs/git-instructions.md
- @~/.claude/my-project-instructions.md
```

## Prompt pattern to start a task
```
Please implement the feature described in PRD-foo.md.
1) Use the generate-tasks subagent to produce a task list.
2) Follow CLAUDE.md for coding conventions and workflow.
3) Write code, add unit tests, run Playwright MCP browser tests.
4) Open a PR when all tests pass.
```
