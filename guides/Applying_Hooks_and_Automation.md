# Applying Hooks and Workflow Automation in Claude Code

## What are Claude Code Hooks
Hooks are event-driven automation points that execute custom commands at specific stages of Claude Code's workflow. They enable sophisticated workflow control, validation, logging, and team integration patterns.

## Hook Event Types

### Core Events
- **PreToolUse**: Execute before any tool runs - perfect for validation and permission checks
- **PostToolUse**: Execute after tool completion - ideal for logging and follow-up actions
- **UserPromptSubmit**: Triggered when users submit prompts - enables context injection
- **Notification**: Responds to system notifications - useful for alerting and monitoring

### Session Events
- **SessionStart**: Runs when Claude Code session begins
- **SessionEnd**: Runs when Claude Code session ends
- **Stop**: Runs when main agent response completes
- **SubagentStop**: Runs when subagent completes tasks

## Hook Configuration Format

### Basic Structure
```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here"
          }
        ]
      }
    ]
  }
}
```

### Tool Matchers
- **Specific tools**: `"Edit"`, `"Bash"`, `"Write"`
- **Tool patterns**: `"Bash(git *)"`, `"Edit(*.ts)"`
- **All tools**: `"*"`
- **Multiple patterns**: `["Edit", "Write", "MultiEdit"]`

## Practical Hook Patterns

### 1. Quality Gates (PreToolUse)
Prevent problematic operations before they execute:
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(rm *)",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'BLOCKED: Dangerous rm command prevented' && exit 1"
          }
        ]
      },
      {
        "matcher": "Edit(*.production.*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'WARNING: Editing production files requires approval' && read -p 'Continue? (y/n): ' confirm && [[ $confirm == y ]]"
          }
        ]
      }
    ]
  }
}
```

### 2. Automated Testing (PostToolUse)
Run tests automatically after code changes:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": ["Edit(*.js)", "Edit(*.ts)", "Write(*.js)", "Write(*.ts)"],
        "hooks": [
          {
            "type": "command",
            "command": "npm run test:quick || echo 'Tests failed - review changes'"
          }
        ]
      },
      {
        "matcher": "Edit(package.json)",
        "hooks": [
          {
            "type": "command",
            "command": "npm install && echo 'Dependencies updated'"
          }
        ]
      }
    ]
  }
}
```

### 3. Team Notifications (PostToolUse)
Notify team members of important changes:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash(git commit *)",
        "hooks": [
          {
            "type": "command",
            "command": "slack-notify 'New commit by Claude Code: $(git log -1 --pretty=format:\"%s\")'"
          }
        ]
      },
      {
        "matcher": "Edit(src/api/*)",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'API change detected - updating documentation' && npm run docs:generate"
          }
        ]
      }
    ]
  }
}
```

### 4. Context Injection (UserPromptSubmit)
Automatically add context to user prompts:
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Current branch:' $(git branch --show-current) '| Last commit:' $(git log -1 --pretty=format:'%h %s')"
          }
        ]
      }
    ]
  }
}
```

## Advanced Automation Patterns

### Multi-Stage Workflows
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git push *)",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lint && npm run test && npm run build"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash(git push *)",
        "hooks": [
          {
            "type": "command",
            "command": "slack-notify 'Code pushed to $(git branch --show-current) - CI/CD pipeline started'"
          }
        ]
      }
    ]
  }
}
```

### Conditional Logic
```bash
# Complex validation script
#!/bin/bash
# validate-edit.sh
FILE=$1
if [[ $FILE == *.production.* ]]; then
  echo "Production file detected: $FILE"
  if ! git diff --quiet HEAD; then
    echo "ERROR: Uncommitted changes exist. Commit before editing production files."
    exit 1
  fi
  echo "Validation passed for production edit"
fi
```

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit(*)",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/validate-edit.sh"
          }
        ]
      }
    ]
  }
}
```

## Enterprise Integration Patterns

### 1. Security Compliance
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": ["Edit(*.env*)", "Write(*.env*)"],
        "hooks": [
          {
            "type": "command",
            "command": "echo 'SECURITY: Environment file changes require approval' && security-check-env-file"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash(git commit *)",
        "hooks": [
          {
            "type": "command",
            "command": "git-secrets --scan && echo 'Security scan passed'"
          }
        ]
      }
    ]
  }
}
```

### 2. Audit Logging
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date): USER_PROMPT: $CLAUDE_USER_MESSAGE\" >> audit.log"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$(date): TOOL_EXECUTED: $CLAUDE_TOOL_NAME\" >> audit.log"
          }
        ]
      }
    ]
  }
}
```

### 3. Development Workflow Automation
```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "git fetch && echo 'Repository updated' && npm install && echo 'Dependencies ready'"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash(npm run build)",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lighthouse && echo 'Performance audit complete'"
          }
        ]
      }
    ]
  }
}
```

## Hook Configuration Management

### Project-Level Hooks
```bash
# Store in .claude/settings/hooks.json
mkdir -p .claude/settings
cat > .claude/settings/hooks.json << 'EOF'
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit(src/**/*.ts)",
        "hooks": [
          {
            "type": "command",
            "command": "npm run type-check"
          }
        ]
      }
    ]
  }
}
EOF
```

### User-Level Hooks
```bash
# Store in ~/.claude/settings/hooks.json
mkdir -p ~/.claude/settings
# Global hooks apply to all projects
```

### Environment-Specific Hooks
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git push origin main)",
        "hooks": [
          {
            "type": "command",
            "command": "[[ \"$NODE_ENV\" == \"production\" ]] && echo 'Production push blocked in dev environment' && exit 1 || echo 'Dev push allowed'"
          }
        ]
      }
    ]
  }
}
```

## Best Practices

### Performance Considerations
- **Keep hook commands fast** - they block Claude Code execution
- **Use background processes** for long-running tasks (`command &`)
- **Cache results** when possible to avoid repeated work
- **Fail fast** - exit early on validation errors

### Error Handling
- **Non-zero exit codes** block tool execution in PreToolUse hooks
- **Log errors clearly** for debugging hook issues
- **Provide fallback behavior** when external services are unavailable
- **Test hooks thoroughly** before deploying to team

### Security Guidelines
- **Validate all inputs** - hooks receive external data
- **Limit hook permissions** - don't run hooks as root
- **Audit hook changes** - treat hook configs as critical code
- **Use secure communication** for external integrations

## Workshop Exercises

1. **Quality Gate Setup**: Create PreToolUse hooks that prevent dangerous operations
2. **Test Automation**: Set up PostToolUse hooks for automatic testing
3. **Team Integration**: Configure hooks for Slack/Discord notifications
4. **Security Compliance**: Implement hooks for secret scanning and audit logging

See `/examples/hooks/` for detailed configuration examples and use cases.