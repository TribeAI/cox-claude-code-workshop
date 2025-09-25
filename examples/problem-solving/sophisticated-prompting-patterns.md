# Sophisticated Prompting Patterns for Complex Scenarios

## Beyond Basic Commands
Advanced Claude Code usage isn't about knowing more features—it's about crafting prompts that get superior results for complex, real-world scenarios.

## Power User Prompting Patterns

### Pattern 1: Constraint-Driven Development
Use constraints to force better solutions and faster results.

```bash
# Basic prompt (often leads to over-engineering)
You: "Add error handling to the API calls"

# Constraint-driven prompt (forces focused solution)
You: "Add error handling to the API calls using only React built-ins, under 30 lines total, with user-friendly messages for 3 specific error types: network, auth, and validation"

Result: Claude produces focused, practical error handling instead of complex error management systems
```

```bash
# Complex feature with constraints
You: "Add user settings with these constraints:
- Use existing form validation patterns from ContactForm.jsx
- Under 100 lines of new code total
- Must work with current Redux state structure (don't modify userSlice.js)
- 3 settings max: theme, notifications, language"

Result: Focused implementation that integrates cleanly with existing code
```

### Pattern 2: Result-Driven Prompting
Start with the desired outcome, work backwards to implementation.

```bash
# Basic prompt (unclear success criteria)
You: "Improve the search functionality"

# Result-driven prompt (clear success definition)
You: "I want users to find todos in under 2 seconds. Current search is slow with 100+ todos. Success criteria:
1. Search results appear as user types (no search button)
2. Highlights matching text in results
3. Works with current TodoList component unchanged
4. Handles typos (fuzzy matching)

Show me the fastest way to achieve this."

Result: Claude focuses on performance and user experience, not just feature addition
```

### Pattern 3: Pattern-Based Delegation
Leverage existing code patterns for consistency and speed.

```bash
# Instead of explaining everything
You: "Add todo categories using the same pattern as todo priorities in TodoItem.jsx:
- Same data structure approach (enum + display mapping)
- Same styling approach (color-coded badges)
- Same filter implementation pattern as PriorityFilter.jsx
- Reuse existing dropdown component"

Result: Consistent implementation that follows established patterns
```

### Pattern 4: Multi-Phase Problem Solving
Break complex problems into phases with validation gates.

```bash
# Phase 1: Analysis
You: "Analyze why the TodoApp renders slowly with 500+ todos. Profile the performance and identify the top 3 bottlenecks. Don't implement fixes yet."

# [Wait for analysis, validate findings]

# Phase 2: Targeted Solutions
You: "Based on your analysis, implement fixes for the 2 most impactful bottlenecks only. Measure before/after performance."

# [Wait for implementation, test results]

# Phase 3: Validation
You: "Run performance tests with 1000 todos and confirm we hit our target of <100ms render time."

Result: Systematic problem-solving with measurable outcomes
```

## Advanced Prompting Techniques

### Technique 1: Assumption Validation
Make implicit assumptions explicit to avoid mismatched expectations.

```bash
# Bad - lots of hidden assumptions
You: "Add authentication to the app"

# Good - explicit assumptions
You: "Add JWT-based authentication assuming:
- Backend already exists at /api/auth/login
- Users have email/password credentials
- Token stored in localStorage
- 401 responses should redirect to login
- Current routing uses React Router v6

Confirm these assumptions match your implementation approach."
```

### Technique 2: Error Prevention
Anticipate common failure modes and address them upfront.

```bash
You: "Add drag-and-drop to reorder todos. Common issues to avoid:
- Don't break existing click handlers on todo items
- Maintain keyboard accessibility
- Handle edge cases (dragging to same position)
- Work with current list filtering (don't reorder filtered items)
- Keep existing todo completion/deletion features intact

Use a lightweight approach - no external libraries."
```

### Technique 3: Context Priming
Set up the right mental model before complex tasks.

```bash
You: "You're working on a production React app used by 10,000+ users daily. Performance and reliability matter more than fancy features.

The TodoApp currently works well but users want categories. This is a high-impact feature that needs to ship bug-free in 2 weeks.

Approach: Add minimal viable categories feature first, then enhance. Implementation should be:
- Conservative (don't break existing functionality)
- Performant (no render regressions)
- Tested (include basic test coverage)

Add basic todo categories now."

Result: Claude adopts production mindset vs demo/prototype mindset
```

## Scenario-Specific Patterns

### Complex Refactoring
```bash
You: "Refactor TodoApp from class to functional components with these requirements:
1. Zero behavior changes (existing tests must still pass)
2. Convert one component per response (don't try to do everything at once)
3. Start with the leaf components (no child components) first
4. Show me the conversion plan before starting
5. After each conversion, confirm the app still works

Which component should we convert first and why?"

Result: Systematic, safe refactoring approach
```

### Performance Optimization
```bash
You: "The TodoList component re-renders too much. Debug this systematically:

Step 1: Add React DevTools Profiler measurements for current performance
Step 2: Identify which props/state changes trigger unnecessary renders
Step 3: Show me the top 3 optimization opportunities (don't implement yet)
Step 4: Implement the #1 optimization only and measure improvement
Step 5: Validate no functionality is broken

Start with step 1."

Result: Data-driven optimization vs guesswork
```

### Integration Challenges
```bash
You: "Integrate with external payment API with these constraints:
- The API is unreliable (fails 5% of the time)
- Users expect immediate feedback
- Failed payments must be retryable
- No user data can be lost
- Must work offline-first (queue payments when offline)

Design the integration strategy before implementing. Consider error cases, user experience, and data consistency."

Result: Robust integration design vs happy-path-only implementation
```

## Workshop-Optimized Prompting

### Time-Boxed Implementation
```bash
You: "We have 15 minutes to add basic todo search. Priority order:
1. Working search input that filters visible todos (5 min)
2. Highlight matching text in results (5 min)
3. Search within completed todos too (3 min)
4. If time remains: keyboard shortcuts (2 min)

Implement in priority order. Stop at 15 minutes regardless of completion status."

Result: Focuses on essential functionality first
```

### Learning-Focused Prompting
```bash
You: "Add todo due dates to teach me React patterns. For each implementation decision, explain:
- Why you chose this approach over alternatives
- What React principles it demonstrates
- How it would scale to more complex features
- What could go wrong and how to avoid it

Show me both the code AND the reasoning."

Result: Educational implementation vs just working code
```

### Demo-Safe Prompting
```bash
You: "Create a demo-ready feature that shows off Claude Code capabilities:
- Must work reliably (no edge case bugs)
- Should look polished (basic styling)
- Demonstrates multiple Claude Code skills (file reading, editing, testing)
- Has clear before/after comparison
- Takes 5 minutes total to implement

Suggest 3 demo-worthy features and implement the most impressive one."

Result: Reliable demo content vs potentially buggy complex features
```

## Anti-Patterns to Avoid

### Anti-Pattern 1: Vague Requirements
```bash
# Bad
You: "Make the app better"

# Good
You: "Improve user experience for users with 50+ todos by addressing the top 2 pain points: finding specific todos and managing completed items"
```

### Anti-Pattern 2: Solution Prescriptive
```bash
# Bad
You: "Use React.memo and useMemo to optimize performance"

# Good
You: "Optimize rendering performance. Profile first, then apply the most effective optimizations for this specific use case"
```

### Anti-Pattern 3: Feature Creep Mid-Task
```bash
# Bad
You: "Add categories"
[Mid-implementation]: "Also add tags and priority levels"
[Later]: "Make categories hierarchical"

# Good
You: "Add basic categories first. We'll enhance with additional features in separate tasks after this works well"
```

## Workshop Exercises

### Exercise 1: Constraint Mastery
1. Take a complex feature request
2. Add 3-4 meaningful constraints that force better design
3. Compare results vs unconstrained implementation
4. **Success**: Constrained version is cleaner and more focused

### Exercise 2: Multi-Phase Problem Solving
1. Pick a performance or complexity issue
2. Break into analysis → solution → validation phases
3. Practice waiting between phases for validation
4. **Success**: Systematic approach produces better results than all-at-once

### Exercise 3: Context Priming
1. Practice setting up the right "mindset" for different scenarios
2. Compare results: prototype mindset vs production mindset
3. Test educational prompting vs just-get-it-done prompting
4. **Success**: Match prompting approach to actual goals

These sophisticated prompting patterns turn Claude Code into a true development partner that understands not just what you want to build, but how and why you want to build it.