# Applying Git Worktrees in Claude Code

## What are Git Worktrees
Git worktrees allow you to have multiple working directories from a single Git repository. Each worktree can have different branches checked out simultaneously, enabling parallel development without branch switching overhead.

## Why Use Worktrees with Claude Code
- **Parallel Development**: Run multiple Claude Code sessions on different features simultaneously
- **Safe Experimentation**: Test risky changes in isolated worktrees while keeping main work intact
- **Efficient Context Switching**: No need to stash/commit work when switching between features
- **Collaborative Workflows**: Team members can work on different branches without conflicts

## Setting Up Git Worktrees

### Prerequisites
Ensure your settings allow worktree operations:
```json
{
  "permissions": {
    "allow": [
      "Bash(git worktree:*)"
    ]
  }
}
```

### Basic Worktree Commands
```bash
# List existing worktrees
git worktree list

# Create new worktree for feature branch
git worktree add ../feature-branch-name feature-branch-name

# Create worktree with new branch
git worktree add ../new-feature -b new-feature

# Remove worktree when done
git worktree remove ../feature-branch-name
```

## Claude Code Worktree Workflows

### Pattern 1: Feature Development Isolation
```bash
# Main development directory
cd ~/project-main
# Claude Code session 1: main development

# Feature worktree
git worktree add ../project-feature -b feature/user-auth
cd ../project-feature
# Claude Code session 2: feature development
```

### Pattern 2: Hotfix Management
```bash
# Continue development in main
cd ~/project
# Claude Code for ongoing work

# Emergency hotfix in separate worktree
git worktree add ../hotfix -b hotfix/security-patch
cd ../hotfix
# Claude Code for urgent fix - no disruption to main work
```

### Pattern 3: Experimental Branches
```bash
# Safe experimentation
git worktree add ../experiment -b experiment/new-architecture
cd ../experiment
# Claude Code for risky refactoring - easy to abandon if needed
```

## Advanced Worktree Patterns

### Coordinated Agent Development
Use specialized agents in different worktrees:
```bash
# Backend worktree with backend-focused agent
git worktree add ../backend-work -b feature/api-endpoints
# Frontend worktree with UI-focused agent
git worktree add ../frontend-work -b feature/user-interface
```

### Testing Strategy Separation
```bash
# Development worktree
git worktree add ../dev-branch -b feature/implementation
# Testing worktree (same feature, focus on tests)
git worktree add ../test-branch feature/implementation
```

## Best Practices

### Directory Organization
```
project/                    # Main repository
├── .git/                  # Git metadata
├── src/                   # Main branch files
project-feature/           # Feature worktree
├── src/                   # Feature branch files
project-hotfix/           # Hotfix worktree
├── src/                   # Hotfix branch files
```

### Claude Code Session Management
- **One Claude session per worktree** for clean separation
- **Use descriptive worktree names** matching branch names
- **Set different CLAUDE.md** contexts per worktree if needed
- **Coordinate agents** - avoid resource conflicts between sessions

### Integration Workflow
1. **Develop in parallel** across worktrees
2. **Test integration** by merging into temporary branch
3. **Coordinate PRs** - sequence merges appropriately
4. **Clean up worktrees** after features complete

## Troubleshooting

### Common Issues
- **Shared dependencies**: Node modules, build artifacts may conflict
- **Port conflicts**: Multiple dev servers need different ports
- **Lock files**: Package managers may create conflicts

### Solutions
```bash
# Different ports per worktree
cd ../feature-worktree
PORT=3001 npm start

# Separate node_modules per worktree
npm install  # Run in each worktree directory

# Clean up unused worktrees
git worktree prune
```

## Workshop Exercise
1. Create a feature worktree for TodoMVC enhancement
2. Run Claude Code in both main and feature directories
3. Develop different aspects of the same feature simultaneously
4. Practice merging and cleanup workflows

See examples in `/examples/worktrees/` for detailed scenarios and commands.