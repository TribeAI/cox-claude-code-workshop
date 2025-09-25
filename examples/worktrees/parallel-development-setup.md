# Parallel Development with Git Worktrees

## Quick Setup for Workshop

### Scenario
You need to add **dark mode** and **user preferences** to TodoMVC simultaneously, without blocking each other.

### 5-Minute Setup

```bash
# In your TodoMVC directory
cd todomvc/examples/react

# Create parallel worktrees
git worktree add ../react-darkmode -b feature/dark-mode
git worktree add ../react-preferences -b feature/user-preferences

# Verify setup
git worktree list
```

### Development Workflow

#### Terminal 1: Dark Mode Development
```bash
cd ../react-darkmode
npm install
PORT=3001 npm start &

# Claude Code session focused on dark mode
# Context: "Focus only on theme switching - CSS variables, toggle component, localStorage persistence"
```

#### Terminal 2: Preferences Development
```bash
cd ../react-preferences
npm install
PORT=3002 npm start &

# Claude Code session focused on preferences
# Context: "Focus only on user settings - preferences data structure, settings panel UI, persistence"
```

#### Integration Strategy
```bash
# After both features work independently
cd todomvc/examples/react
git merge feature/dark-mode
git merge feature/user-preferences
# Resolve any conflicts, test integration
```

## Advanced Patterns

### Pattern 1: Feature Isolation
Each worktree develops one focused feature without distractions.

```bash
# Dark Mode Worktree - Focused Context
"Add dark mode toggle to TodoMVC:
1. CSS variables for theming
2. Toggle button in header
3. Save preference to localStorage
4. No other changes - keep scope tight"

# Preferences Worktree - Focused Context
"Add user preferences to TodoMVC:
1. Preferences data structure (theme, display options)
2. Settings modal/panel
3. Save/load from localStorage
4. No styling changes - focus on functionality"
```

### Pattern 2: Coordination Points
Define clear interfaces between parallel development streams.

```bash
# Before starting development
"Both features will interact through:
1. Shared localStorage key structure: user.preferences.theme
2. Shared CSS variable naming: --color-bg, --color-text
3. Shared component prop interfaces: theme={light|dark}

Document these interfaces first, then develop features independently."
```

### Pattern 3: Specialized Agents per Worktree
Use different agent contexts for different focuses.

```bash
# Dark Mode Worktree - UI/UX focused agent
"You're a UI/UX specialist. Focus on:
- Clean theme switching without flash
- Accessibility (color contrast, system preferences)
- Smooth visual transitions
- Minimal performance impact"

# Preferences Worktree - Data/State focused agent
"You're a state management specialist. Focus on:
- Clean data structures
- Efficient localStorage usage
- Preference validation and defaults
- Backward compatibility"
```

## Real-World Benefits

### For Teams
- **No Blocking**: Frontend and backend developers work simultaneously
- **Clean History**: Separate branches keep commits focused
- **Easy Testing**: Test features independently before integration
- **Risk Isolation**: One feature's issues don't block the other

### For Solo Developers
- **Context Switching**: Jump between different types of work without mental overhead
- **Experimentation**: Try risky approaches without affecting main work
- **Focus**: Each worktree maintains focused context and avoid scope creep

## Workshop Exercise

### Setup (5 minutes)
1. Create 2 worktrees for TodoMVC with different features
2. Start development servers on different ports
3. Open separate Claude Code sessions

### Parallel Development (15 minutes)
1. **Worktree 1**: Add todo categories (data structure + basic UI)
2. **Worktree 2**: Add keyboard shortcuts (event handlers + help modal)
3. Keep features completely independent

### Integration (5 minutes)
1. Merge both features into main branch
2. Test combined functionality
3. Resolve any integration issues

### Success Criteria
- Both features work independently
- Integration happens without major conflicts
- You can switch contexts without losing focus
- Faster overall development than sequential approach

## Common Gotchas

### Port Conflicts
```bash
# Run dev servers on different ports
PORT=3001 npm start  # First worktree
PORT=3002 npm start  # Second worktree
```

### Package Dependencies
```bash
# Run npm install in each worktree
cd ../react-feature1 && npm install
cd ../react-feature2 && npm install
```

### Context Management
```bash
# Use different CLAUDE.md files per worktree if needed
# Keep focused contexts - don't let features bleed into each other
```

### Integration Planning
```bash
# Plan integration points before starting
# Define shared interfaces and data structures
# Test integration frequently (daily merges if possible)
```

This pattern scales from simple parallel features to complex multi-team development workflows.