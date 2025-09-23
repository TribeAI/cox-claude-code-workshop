# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

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
3. Code Review (code-reviewer) →
4. Security Review (security-code-reviewer)
```

**IMPORTANT**: Never skip the code-reviewer and security-code-reviewer steps. They are mandatory for all completed work.

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