# Debugging Stuck Claude Code Workflows

## Common Scenarios When Claude Gets Stuck

### 1. Context Overload
**Problem**: Claude loses track in large codebases or complex conversations
```
You: "Update the user authentication system to support OAuth2"
Claude: "I need to understand your current authentication system first..."
[Reads 20 files, gets confused about which approach to take]
```

**Solution Pattern**:
```
You: "Focus only on src/auth/login.js. Add OAuth2 provider support to this specific file. Don't read other auth files yet."
Claude: [Focused response, clear implementation]

Then: "Now integrate this with src/auth/providers.js using the pattern you just created."
```

### 2. Ambiguous Requirements
**Problem**: Claude asks clarifying questions instead of making progress
```
You: "Improve the performance of this component"
Claude: "What specific performance issues are you experiencing? Should I focus on rendering, memory usage, or bundle size?"
[Workshop time wasted on back-and-forth]
```

**Solution Pattern**:
```
You: "Improve rendering performance of UserList.jsx. Profile it first, then optimize the 3 slowest operations you find. Show before/after metrics."
Claude: [Specific actions, measurable results]
```

### 3. Analysis Paralysis
**Problem**: Claude over-analyzes instead of implementing
```
You: "Add dark mode to the app"
Claude: [Writes 500 words about CSS-in-JS vs CSS variables vs styled-components approaches]
```

**Solution Pattern**:
```
You: "Add dark mode using CSS variables. Start with a working toggle in App.jsx, then add dark styles for 3 main components. Implementation first, optimization later."
Claude: [Concrete implementation, iterative approach]
```

## Advanced Troubleshooting Techniques

### Reset and Redirect
When Claude is confused:
```bash
# Bad approach - continuing confused conversation
You: "No, that's not what I meant..."

# Good approach - clean reset with clear context
You: "Stop. Let's start over. Here's exactly what I need:
1. Edit src/components/TodoItem.jsx only
2. Add a 'priority' prop (high/medium/low)
3. Style high priority items with red border
4. Test it works with the existing TodoList component"
```

### Scope Limiting
When Claude tries to do too much:
```bash
# Bad - vague scope
You: "Refactor the authentication system"

# Good - limited scope with clear boundaries
You: "Refactor ONLY the login form in src/auth/LoginForm.jsx:
- Replace class component with hooks
- Add TypeScript types
- Keep all existing functionality identical
- Don't modify any other auth files"
```

### Progressive Disclosure
When dealing with complex changes:
```bash
# Phase 1: Foundation
You: "Add user preferences data structure to the existing TodoApp state"
[Wait for completion]

# Phase 2: UI
You: "Now add a preferences panel component that reads/writes those preferences"
[Wait for completion]

# Phase 3: Integration
You: "Connect the preferences to change the TodoList display (theme, sorting, etc.)"
```

## Workshop-Specific Recovery Patterns

### When Demo Goes Wrong
**Problem**: Live demo fails, workshop stalls
**Recovery**:
```bash
# Quick recovery command
You: "Skip the complex part. Show me the simplest working version of this feature in 2 minutes."

# Or fallback to working example
You: "Use the pattern from examples/hooks/pre-commit-hooks.json but adapt it for our current task."
```

### Time Management
**Problem**: Complex task eating up workshop time
**Recovery**:
```bash
# Time-boxed approach
You: "We have 10 minutes left. Give me the minimal working version now, then list the 3 most important improvements as TODO comments."

# Focus on learning outcome
You: "The goal is to learn the pattern, not perfect implementation. Show me a working example with clear comments explaining the approach."
```

### When Code Doesn't Work
**Problem**: Generated code has bugs
**Recovery**:
```bash
# Systematic debugging
You: "Debug this step by step:
1. Run the failing test/command and show exact error
2. Identify the specific line causing the issue
3. Fix just that one issue
4. Verify it works before adding more features"
```

## Advanced Context Management

### Conversation Checkpoints
Create explicit context anchors:
```bash
You: "Before we continue - summarize what we've built so far in 3 bullet points."
Claude: [Summary creates shared context checkpoint]
You: "Good. Now let's add the next feature..."
```

### Context Injection
Add relevant context proactively:
```bash
# Instead of letting Claude search/guess
You: "I'm using React 18 with TypeScript in strict mode. The existing TodoApp uses useReducer for state management. Here's the current interface: [paste relevant types]. Now add..."
```

### Scope Validation
Confirm understanding before complex tasks:
```bash
You: "Before you start coding, tell me:
1. Which files will you modify?
2. What's the main approach you'll use?
3. How will you test it works?"

[Wait for confirmation, correct if needed, then proceed]
```

## Power User Patterns

### Rapid Iteration
```bash
# Pattern: Quick → Test → Improve
You: "Quick working version first: Add a simple delete button to each todo item"
[Get working version]
You: "Now improve it: Add confirmation dialog and undo functionality"
[Iterative enhancement]
```

### Constraint-Driven Development
```bash
# Force specific approaches for learning
You: "Solve this using ONLY React hooks, no external libraries, under 50 lines of code"
[Forces creative, focused solutions]
```

### Pattern Extraction
```bash
# Learn reusable patterns
You: "Extract the reusable pattern from what you just built and show me how to apply it to 2 other similar problems"
[Builds transferable knowledge]
```

## Workshop Exercise: Debugging Practice

**Setup**: Intentionally create a "stuck" scenario
1. Ask Claude to implement a complex feature with vague requirements
2. Let it get confused/over-analyze for 2 minutes
3. Practice recovery techniques:
   - Reset and redirect
   - Scope limiting
   - Progressive disclosure

**Success Criteria**:
- Go from confused/stuck to working implementation in under 5 minutes
- Learn 2-3 recovery patterns that apply to your daily workflow