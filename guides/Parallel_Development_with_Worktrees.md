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

### Advanced Setup with Clean Context Management
```bash
# Frontend-focused worktree with clear directory naming
git worktree add ../project-frontend -b feature/frontend-work
cd ../project-frontend
# Use terminal title and conversational context instead of files
echo -e "\033]0;Frontend Development - $(basename $(pwd))\007"

# Backend-focused worktree with clear directory naming
git worktree add ../project-backend -b feature/backend-work
cd ../project-backend
# Use terminal title and conversational context instead of files
echo -e "\033]0;Backend Development - $(basename $(pwd))\007"
```

## Claude Code Integration Patterns

### Pattern 1: Context Management Without Configuration Pollution
Maintain focus across worktrees without creating merge conflicts.

```bash
# Use descriptive terminal titles for context
# Terminal 1: Frontend Development
export PS1="[FRONTEND] \u@\h:\w$ "

# Terminal 2: Backend Development
export PS1="[BACKEND] \u@\h:\w$ "

# Start Claude Code with initial context
# "Focus on React components, styling, and UI interactions only"
# "Focus on API endpoints, database, and business logic only"
```

```bash
# Create temporary, gitignored notes for complex contexts
echo "notes/" >> .gitignore
mkdir -p notes

# Frontend worktree context (not committed)
cat > notes/frontend-context.md << 'EOF'
# Frontend Development Notes
- Working on React component architecture
- Focus: src/components/, src/styles/, src/hooks/
- Avoid: API changes, database modifications
- Current sprint: User interaction patterns
EOF
```

### Pattern 2: Session-Based Context Management
Use session management and clear communication instead of configuration files.

```bash
# Start frontend session with clear conversational context
# "I'm working in the frontend worktree on React components and styling.
#  Please focus on UI/UX concerns and assume the API already exists."

# Start backend session with clear conversational context
# "I'm working in the backend worktree on API endpoints and database logic.
#  Please focus on data and business logic, assuming the frontend will consume it correctly."

# Use session naming for terminal multiplexers
tmux new-session -d -s frontend-dev -c ../project-frontend
tmux new-session -d -s backend-dev -c ../project-backend
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

## Context Management Without Configuration Pollution

### The Problem with Per-Worktree Configuration
Creating worktree-specific `CLAUDE.md`, agent configs, or context files causes:
- **Merge conflicts** when integrating branches back to main
- **Polluted git history** with temporary configuration files
- **Inconsistent behavior** across development environments
- **Lost configurations** when merging or cleaning up worktrees

### Clean Context Management Strategies

#### 1. Terminal and Session Organization
```bash
# Use descriptive terminal titles
echo -e "\033]0;Frontend: Components & Styling\007"
echo -e "\033]0;Backend: API & Database\007"

# Organize with terminal multiplexers
tmux new-session -d -s frontend-work -c ../project-frontend
tmux new-session -d -s backend-work -c ../project-backend

# Use shell prompts for context
export PS1="[FRONTEND] \u@\h:\w$ "
export PS1="[BACKEND] \u@\h:\w$ "
```

#### 2. Conversational Context Setup
```bash
# Start Claude Code sessions by clearly stating your focus:
# "I'm working on the frontend in this worktree. Focus on React components,
#  CSS styling, and user interactions. Assume the API is already built."

# "I'm working on the backend worktree. Focus on database models, API endpoints,
#  and business logic. Don't worry about UI implementation."
```

#### 3. Temporary Notes (Gitignored)
```bash
# Add to .gitignore once for the entire project
echo "worktree-notes/" >> .gitignore
mkdir -p worktree-notes

# Create temporary context files that won't be committed
cat > worktree-notes/frontend-context.md << 'EOF'
# Current Frontend Focus
- Component architecture refactoring
- CSS-in-JS implementation
- React Router setup
- Testing component interactions

## Avoiding
- API endpoint changes
- Database schema modifications
- Backend business logic
EOF
```

#### 4. Branch Naming for Self-Documenting Context
```bash
# Use descriptive branch names that indicate focus
git worktree add ../project-ui -b feature/ui-components-redesign
git worktree add ../project-api -b feature/api-authentication-system
git worktree add ../project-db -b feature/database-migration-v2

# Directory names become context clues
ls ../project-*
# project-ui/     -> Frontend work
# project-api/    -> Backend API work
# project-db/     -> Database work
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
1. **Use consistent project configuration** - Keep CLAUDE.md shared across all worktrees
2. **Conversational context setup** - Start sessions with clear focus statements
3. **Clear boundaries** - Define what's in/out of scope for each worktree conversationally
4. **Document decisions** - Track important choices in gitignored notes or issues

### Performance Optimization
1. **Share dependencies** - Use symlinks for node_modules when possible
2. **Different ports** - Run dev servers on different ports for parallel testing
3. **Resource management** - Don't run heavy processes in all worktrees simultaneously
4. **Clean up regularly** - Remove completed worktrees to save disk space

This approach transforms development from sequential context switching into focused, parallel problem-solving that matches how you naturally think about complex features.