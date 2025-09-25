# Parallel Development with Git Worktrees

## Why Worktrees for Claude Code?

Git worktrees solve a unique problem for Claude Code users: **context switching overhead**. When you switch branches, Claude loses context about your current work. Worktrees let you maintain multiple focused Claude Code sessions simultaneously.

## Core Concept

Instead of:
```bash
git checkout feature/dark-mode    # Lose context
# Work with Claude on dark mode
git checkout feature/preferences  # Lose context again
# Work with Claude on preferences
```

You get:
```bash
# Two parallel, focused development environments
react-darkmode/     # Claude session 1: Dark mode context
react-preferences/  # Claude session 2: Preferences context
```

## When to Use Worktrees

### Ideal Scenarios
- **Parallel Features**: Frontend + Backend work on same feature
- **Experiment + Stable**: Try risky refactoring while maintaining working version
- **Review + Development**: Review PRs while continuing development
- **Multiple Contexts**: Different types of work requiring different mental models

### Not Worth It For
- Simple, sequential tasks
- Short-lived branches (< 1 hour of work)
- When features have heavy interdependencies

## Setup Pattern

### Basic Setup
```bash
# From your main project directory
git worktree add ../project-feature -b feature/new-feature
cd ../project-feature
# Setup development environment (npm install, etc.)
# Start Claude Code session with focused context
```

### Advanced Setup with Specialized Contexts
```bash
# Frontend-focused worktree
git worktree add ../project-frontend -b feature/frontend-work
cd ../project-frontend
echo "Focus: UI components, styling, user interaction" > CLAUDE_CONTEXT.md

# Backend-focused worktree
git worktree add ../project-backend -b feature/backend-work
cd ../project-backend
echo "Focus: API design, database, business logic" > CLAUDE_CONTEXT.md
```

## Claude Code Integration Patterns

### Pattern 1: Focused Context Files
Create worktree-specific context to keep Claude focused.

```bash
# In frontend worktree
cat > CLAUDE.md << 'EOF'
# Frontend Development Context

## Current Focus
- React component development
- CSS/styling implementation
- User interaction patterns
- No backend changes

## Key Files
- src/components/ (React components)
- src/styles/ (CSS modules)
- src/hooks/ (Custom hooks)

## Constraints
- Keep API calls in existing services
- Follow existing component patterns
- Test components in isolation
EOF
```

```bash
# In backend worktree
cat > CLAUDE.md << 'EOF'
# Backend Development Context

## Current Focus
- API endpoint development
- Database schema changes
- Business logic implementation
- No frontend changes

## Key Files
- src/api/ (Route handlers)
- src/models/ (Database models)
- src/services/ (Business logic)

## Constraints
- Maintain API compatibility
- Follow existing error handling patterns
- Include appropriate tests
EOF
```

### Pattern 2: Specialized Agent Usage
Use different agents optimized for different contexts.

```bash
# Frontend worktree - UI specialist agent
"You're working in the frontend worktree. Focus purely on:
- Component design and implementation
- CSS and styling decisions
- User experience optimization
- React best practices

Ignore backend concerns - assume APIs exist and work correctly."

# Backend worktree - API specialist agent
"You're working in the backend worktree. Focus purely on:
- API design and implementation
- Database operations and optimization
- Business logic and validation
- Error handling and logging

Ignore frontend concerns - assume components will consume APIs correctly."
```

### Pattern 3: Cross-Worktree Coordination
Coordinate when features need to integrate.

```bash
# After independent development
# In main directory, merge both worktrees
git checkout main
git merge feature/frontend-work
git merge feature/backend-work

# Test integration
npm run test
npm run build

# If conflicts, resolve with both contexts in mind
```

## Workshop-Optimized Workflows

### 30-Minute Parallel Feature Development
```bash
# Minutes 0-5: Setup
git worktree add ../todo-categories -b feature/todo-categories
git worktree add ../todo-search -b feature/todo-search

# Minutes 5-25: Parallel Development
# Terminal 1: Categories (data structure, UI components)
# Terminal 2: Search (filtering logic, search UI)
# Each Claude session stays focused on its specific feature

# Minutes 25-30: Integration
git merge feature/todo-categories
git merge feature/todo-search
# Quick integration test
```

### Experiment + Stable Pattern
```bash
# Keep stable version running
git worktree add ../todo-experiment -b experiment/new-architecture

# Experiment with risky changes without affecting main work
# If experiment works: merge it
# If experiment fails: just delete the worktree
git worktree remove ../todo-experiment
```

## Advanced Coordination Techniques

### Shared Interface Definition
Before starting parallel development, define shared contracts.

```bash
# Create shared interface definitions
mkdir -p shared/interfaces
cat > shared/interfaces/todo-api.ts << 'EOF'
// Shared between frontend and backend worktrees
export interface TodoAPI {
  createTodo: (text: string, category: string) => Promise<Todo>
  updateTodo: (id: string, updates: Partial<Todo>) => Promise<Todo>
  deleteTodo: (id: string) => Promise<void>
}

export interface Todo {
  id: string
  text: string
  completed: boolean
  category: string
  createdAt: string
}
EOF

# Both worktrees implement/consume this interface
```

### Integration Testing Strategy
```bash
# Regular integration checks
# Daily or after major changes
git checkout integration-test
git merge feature/frontend-work
git merge feature/backend-work
npm run test:integration
# If tests pass, continue parallel development
# If tests fail, coordinate to fix interfaces
```

### Cross-Worktree Communication
```bash
# Simple communication via shared files
echo "API endpoint /todos/:id/category implemented" >> ../shared/progress.md
echo "Frontend category selection UI ready for testing" >> ../shared/progress.md

# Or more formal issue tracking
gh issue create --title "Ready for integration: Category API + UI"
```

## Cleanup and Management

### Regular Cleanup
```bash
# List all worktrees
git worktree list

# Remove completed worktrees
git worktree remove ../feature-completed
git branch -d feature/completed-branch

# Prune stale references
git worktree prune
```

### Disk Space Management
```bash
# Worktrees share .git directory but duplicate working files
# Clean up node_modules in unused worktrees
find ../project-* -name node_modules -type d -exec rm -rf {} +

# Or create symlinks for dependencies (advanced)
ln -s ../project-main/node_modules ../project-feature/node_modules
```

## Best Practices

### Development Workflow
1. **Plan interfaces first** - Define shared contracts before parallel work
2. **Keep contexts focused** - Each worktree should have a single, clear purpose
3. **Integrate frequently** - Daily merges prevent large conflicts
4. **Test integration early** - Don't wait until the end to test combined features

### Context Management
1. **Use different CLAUDE.md files** - Keep Claude focused on worktree-specific goals
2. **Specialized prompting** - Adapt prompting style to each worktree's context
3. **Clear boundaries** - Define what's in/out of scope for each worktree
4. **Document decisions** - Track important choices that affect integration

### Performance Optimization
1. **Share dependencies** - Use symlinks for node_modules when possible
2. **Different ports** - Run dev servers on different ports for parallel testing
3. **Resource management** - Don't run heavy processes in all worktrees simultaneously
4. **Clean up regularly** - Remove completed worktrees to save disk space

This approach transforms development from sequential context switching into focused, parallel problem-solving that matches how you naturally think about complex features.