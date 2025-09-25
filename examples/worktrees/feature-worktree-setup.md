# Feature Worktree Setup Example

## Scenario
You're working on TodoMVC and need to implement two features simultaneously:
- **Feature A**: Dark mode toggle
- **Feature B**: Task categories

## Setup Commands

### 1. Check Current Status
```bash
# In your main TodoMVC directory
pwd  # /path/to/todomvc/examples/react
git status
git branch -a
```

### 2. Create Feature Worktrees
```bash
# Create dark mode worktree
git worktree add ../react-dark-mode -b feature/dark-mode
echo "Created dark mode worktree at ../react-dark-mode"

# Create categories worktree
git worktree add ../react-categories -b feature/task-categories
echo "Created categories worktree at ../react-categories"

# Verify worktrees
git worktree list
```

### 3. Set Up Development Environment

#### Main Directory (Ongoing Maintenance)
```bash
cd /path/to/todomvc/examples/react
npm install
# Keep this for bug fixes and main branch work
```

#### Dark Mode Worktree
```bash
cd ../react-dark-mode
npm install
PORT=3001 npm run serve &
echo "Dark mode dev server running on port 3001"
```

#### Categories Worktree
```bash
cd ../react-categories
npm install
PORT=3002 npm run serve &
echo "Categories dev server running on port 3002"
```

## Claude Code Sessions

### Session 1: Dark Mode Development
```bash
cd ../react-dark-mode
# Start Claude Code here
# Focus: Theme switching, CSS variables, localStorage persistence
```

### Session 2: Categories Development
```bash
cd ../react-categories
# Start Claude Code here
# Focus: Category management, filtering, data structure changes
```

### Session 3: Main Branch (Optional)
```bash
cd /path/to/todomvc/examples/react
# Start Claude Code here
# Focus: Bug fixes, documentation updates, integration testing
```

## Development Workflow

### 1. Parallel Development
- **Dark Mode Session**: Implement theme switching logic
- **Categories Session**: Add category CRUD operations
- **Main Session**: Fix any discovered bugs

### 2. Testing Integration
```bash
# Create integration test branch
git checkout main
git checkout -b integration/features
git merge feature/dark-mode
git merge feature/task-categories
# Test combined features
```

### 3. Cleanup After Merge
```bash
# Remove worktrees after features are merged
git worktree remove ../react-dark-mode
git worktree remove ../react-categories

# Clean up branches
git branch -d feature/dark-mode
git branch -d feature/task-categories
```

## Tips for Success

### Resource Management
- **Different ports** prevent dev server conflicts
- **Separate node_modules** avoid version conflicts
- **Independent git state** allows different branch histories

### Claude Code Coordination
- **Use different CLAUDE.md** files with feature-specific context
- **Coordinate agent usage** - don't run intensive operations simultaneously
- **Cross-reference features** when they interact

### Integration Planning
- **Plan merge order** - consider feature dependencies
- **Test combinations** before final integration
- **Coordinate PRs** - may need sequential merging

This setup allows efficient parallel development while maintaining clean separation of concerns.