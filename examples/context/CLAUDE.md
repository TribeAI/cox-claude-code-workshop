# CLAUDE.md

## Purpose
This file provides instructions for Claude Code when working in this repository. Follow these guidelines whenever you are asked to implement features or complete tasks.

## Project Overview

This is a React-based TodoMVC implementation using React 17.0.2. The application follows an MVC-like pattern using React's useReducer hook for state management.

## Architecture

- **Model**: Todo state managed by `todoReducer` in `src/todo/reducer.js`
- **View**: React components in `src/todo/components/`
- **Controller**: Main `App` component using `useReducer` hook in `src/todo/app.jsx`

The application uses a centralized state management approach where:
- All todo actions are dispatched to a single reducer
- State flows down through props to child components
- Components include: Header, Main, Footer, Item, and Input

## Key Files

- `src/todo/app.jsx` - Main application component with useReducer
- `src/todo/reducer.js` - Todo state reducer with actions (ADD_ITEM, UPDATE_ITEM, etc.)
- `src/todo/constants.js` - Action type constants
- `src/todo/components/` - UI components (header, main, footer, item, input)

## General Workflow

Follow this complete workflow when implementing features or completing tasks:

### 1. **Understand the PRD**
- Look for files named `PRD-*.md` in the repository
- Break the requirements into a clear, ordered task list
- Confirm you understand the requirements before coding

### 2. **Generate Tasks with Subagent**
- Use the `task-generator` subagent to produce a structured task plan
- Include coding tasks, test tasks, browser testing, and PR creation steps
- Wait for "Go" confirmation before proceeding with detailed implementation

### 3. **Code Implementation with TDD**
- Follow Test Driven Development (Red-Green-Refactor cycle)
- Work incrementally, committing frequently
- Write clear commit messages (`feat: ...`, `fix: ...`, `test: ...`)
- Follow React and JavaScript best practices

### 4. **Unit Testing**
- Add tests for all new features using existing testing framework
- Use React Testing Library / Jest patterns
- Ensure all tests run locally without errors
- Run `npm run test` frequently during development

### 5. **Browser Testing with MCP**
- Use the Playwright MCP server to run browser tests
- Validate UI interactions (toggles, filters, date pickers)
- Test edge cases and accessibility requirements
- Ensure persistence after refresh

### 6. **Code and Security Review**
- Use `code-reviewer` subagent for code quality review
- Use `security-code-reviewer` subagent for security audit
- Address any issues before proceeding to PR creation

### 7. **PR Creation**
- Use GitHub CLI (`gh`) to open a pull request
- Include PRD file reference in PR description
- Ensure all tests pass before opening PR

## Development Methodology

### Test Driven Development (TDD)

**ALWAYS** follow Test Driven Development when implementing new features or fixing bugs:

1. **Red Phase**: Write a failing test first
   - Create test cases that describe the expected behavior
   - Run `npm run test` to confirm the test fails
   - Tests should be specific and focused on one behavior

2. **Green Phase**: Write minimal code to make the test pass
   - Implement only what's needed to satisfy the test
   - Avoid over-engineering or adding extra features
   - Run `npm run test` to confirm the test passes

3. **Refactor Phase**: Clean up the code while keeping tests green
   - Improve code structure, readability, and performance
   - Ensure all tests continue to pass after refactoring
   - Run `npm run test` frequently during refactoring

#### TDD Guidelines for React Components
- Test component behavior, not implementation details
- Use React Testing Library patterns for user-centric tests
- Test state changes through user interactions
- Mock external dependencies and API calls
- Write integration tests for complex component interactions

#### TDD Guidelines for Reducer Logic
- Test each action type independently
- Verify state transitions are correct
- Test edge cases and error conditions
- Ensure immutability of state updates

## Coding Conventions

Follow these conventions to maintain consistency with the existing codebase:

### React Patterns
- Use **React functional components** with hooks (existing pattern)
- Leverage the **useReducer** hook for state management (follows current architecture)
- Use the existing `todoReducer` pattern for state updates
- Follow the MVC pattern: components → actions → reducer → state

### Data Persistence
- Use **local storage** for persistence (follows existing implementation)
- Maintain state between browser sessions
- Handle storage errors gracefully

### Code Style
- Follow existing repository style for variable names and formatting
- Use existing naming conventions (camelCase for variables, PascalCase for components)
- Maintain consistent indentation and bracket style
- Use descriptive variable and function names

### User Interface
- Write user-friendly UI labels and messages
- Follow existing CSS class patterns from `todomvc-app-css`
- Use `classnames` library for conditional CSS classes (existing dependency)
- Ensure accessibility with proper labels and ARIA attributes

### File Organization
- Place new components in `src/todo/components/`
- Add new actions to `src/todo/constants.js`
- Update reducer logic in `src/todo/reducer.js`
- Follow existing import/export patterns

## Edge Cases to Consider

When implementing features, always consider these edge cases:

### Input Handling
- Empty input validation and user feedback
- Whitespace-only inputs
- Very long text inputs
- Special characters and HTML entities

### Data Management
- Data persistence after browser refresh
- Storage quota exceeded scenarios
- Corrupted localStorage data recovery
- Multiple browser tabs synchronization

### UI Interactions
- Multiple filters toggled simultaneously
- Rapid successive user actions
- Loading states and error handling
- Keyboard navigation support

### Accessibility
- Screen reader compatibility
- Keyboard-only navigation
- Color contrast in dark mode
- Focus management and visual indicators

## Testing Checklist

Before considering any task complete, ensure all items are checked:

### Unit Tests
- [ ] Unit tests for each new function/component
- [ ] Tests cover all code paths and edge cases
- [ ] Tests are isolated and don't depend on external state
- [ ] All existing tests continue to pass

### Integration Tests
- [ ] Integration tests for task creation, editing, and deletion
- [ ] Filter functionality tests (All, Active, Completed)
- [ ] Data persistence tests (localStorage integration)
- [ ] Component interaction tests

### Browser Testing
- [ ] Browser test that mirrors PRD acceptance criteria
- [ ] Cross-browser compatibility (Chrome, Firefox, Safari)
- [ ] Mobile responsive behavior
- [ ] Performance with large datasets

### Code Quality
- [ ] All tests must pass before opening PR (`npm run test`)
- [ ] Code passes linting checks (`npx eslint src/`)
- [ ] No console errors or warnings
- [ ] Code follows established patterns and conventions

## Development Commands

```bash
# Install dependencies
npm install

# Start development server (opens browser automatically)
npm run dev

# Build for production
npm run build

# Serve built files locally on port 7002
npm run serve

# Run tests (CRITICAL: Run frequently during TDD cycles)
npm run test

# Lint code (ESLint with React rules)
npx eslint src/
```

## Subagent Usage Guidelines

### When to Use Subagents

**ALWAYS** use the following subagents at appropriate times during development:

#### Code Review Phase
- **Agent**: `code-reviewer`
- **When**: After completing any task list or significant code changes
- **Purpose**: Review code quality, maintainability, and adherence to React best practices
- **Timing**: Before moving to security review

#### Security Review Phase
- **Agent**: `security-code-reviewer`
- **When**: After code review is complete
- **Purpose**: Identify security vulnerabilities and ensure secure coding practices
- **Timing**: Final step before considering work complete

#### Task Generation
- **Agent**: `task-generator`
- **When**: Starting work on new features or major changes
- **Purpose**: Generate detailed, step-by-step task lists from requirements
- **Timing**: Before beginning implementation

### Subagent Workflow
```
1. Generate Tasks (task-generator) →
2. Implement with TDD →
3. Browser Testing (Playwright MCP) →
4. Code Review (code-reviewer) →
5. Security Review (security-code-reviewer) →
6. PR Creation (GitHub CLI)
```

**IMPORTANT**: This workflow aligns with the General Workflow section. Never skip the review steps - they are mandatory for all completed work.

## Build System

- **Webpack** for bundling with separate dev/prod configurations
- **Babel** for JSX and modern JavaScript transpilation
- **ESLint** for code linting with React-specific rules
- Entry point: `src/index.js`
- Output: `dist/` directory

## Dependencies

- React 17.0.2 with React Router
- classnames for conditional CSS classes
- todomvc-app-css for styling
- Webpack ecosystem for building

## Example Prompt

When starting work on a new feature, use this prompt structure for consistency:

```
Please implement the feature described in PRD-[feature-name].md.

1. Use the task-generator subagent to produce a detailed task list
2. Follow CLAUDE.md for coding conventions and workflow
3. Implement using Test Driven Development (TDD)
4. Write comprehensive unit and integration tests
5. Run Playwright MCP browser tests to validate UI interactions
6. Use code-reviewer and security-code-reviewer subagents for quality assurance
7. Open a PR with GitHub CLI when all tests pass

Ensure all edge cases are covered and follow the Testing Checklist before completion.
```

This approach ensures consistency across all feature implementations and maintains the established quality standards.