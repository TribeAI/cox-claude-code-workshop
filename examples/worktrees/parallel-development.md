# Parallel Development with Git Worktrees

## Advanced Workflow Example

This example demonstrates sophisticated parallel development using multiple worktrees with specialized Claude Code agents.

## Scenario: TodoMVC Enterprise Features

Implementing three complex features simultaneously:
1. **User Authentication** - Login, registration, user sessions
2. **Data Persistence** - Backend API, database integration
3. **Advanced UI** - Drag & drop, keyboard shortcuts, accessibility

## Worktree Strategy

### Repository Structure
```
todomvc-main/              # Main development (stable)
├── src/
├── package.json
├── .claude/
│   ├── agents/
│   └── commands/

todomvc-auth/              # Authentication worktree
├── src/                   # Same codebase, auth branch
├── .claude/
│   ├── agents/
│   │   └── auth-specialist.md
│   └── CLAUDE.md          # Auth-focused context

todomvc-backend/           # Backend worktree
├── src/
├── .claude/
│   ├── agents/
│   │   └── backend-architect.md
│   └── CLAUDE.md          # API-focused context

todomvc-ui/                # UI Enhancement worktree
├── src/
├── .claude/
│   ├── agents/
│   │   └── ux-specialist.md
│   └── CLAUDE.md          # UX-focused context
```

## Setup Commands

### 1. Create Specialized Worktrees
```bash
# Authentication worktree
git worktree add ../todomvc-auth -b feature/authentication
cd ../todomvc-auth
npm install
PORT=3001 npm run serve &

# Backend worktree
git worktree add ../todomvc-backend -b feature/backend-api
cd ../todomvc-backend
npm install
PORT=3002 npm run serve &

# UI Enhancement worktree
git worktree add ../todomvc-ui -b feature/advanced-ui
cd ../todomvc-ui
npm install
PORT=3003 npm run serve &
```

### 2. Configure Specialized Agents

#### Authentication Agent (`todomvc-auth/.claude/agents/auth-specialist.md`)
```markdown
---
name: auth-specialist
description: Expert in authentication, security, and user management systems
tools: Read, Write, Edit, Bash, Grep, Glob
---
You are a security-focused authentication specialist. You excel at:
- Implementing secure login/registration flows
- JWT token management and validation
- Password hashing and security best practices
- Session management and logout functionality
- Integration with authentication providers
- Security vulnerability assessment

Always prioritize security best practices and follow OAuth 2.0/OpenID Connect standards.
```

#### Backend Architect (`todomvc-backend/.claude/agents/backend-architect.md`)
```markdown
---
name: backend-architect
description: Expert in API design, database integration, and backend architecture
tools: Read, Write, Edit, Bash, Grep, Glob
---
You are a backend architecture specialist focused on:
- RESTful API design and implementation
- Database schema design and optimization
- Data persistence and migration strategies
- Error handling and validation
- Performance optimization and caching
- API documentation and testing

Emphasize scalable, maintainable backend solutions with proper separation of concerns.
```

#### UX Specialist (`todomvc-ui/.claude/agents/ux-specialist.md`)
```markdown
---
name: ux-specialist
description: Expert in user experience, accessibility, and advanced UI interactions
tools: Read, Write, Edit, Bash, Grep, Glob
---
You are a UX/accessibility specialist focusing on:
- Advanced user interactions (drag & drop, keyboard navigation)
- Accessibility compliance (WCAG 2.1 guidelines)
- Progressive enhancement and responsive design
- Animation and micro-interactions
- Usability testing and user feedback integration
- Performance optimization for UI components

Always consider accessibility first and ensure inclusive design practices.
```

## Coordinated Development Workflow

### Phase 1: Foundation (Week 1)
```bash
# Authentication Worktree
cd ../todomvc-auth
# Claude Code Session 1: "Set up user authentication foundation with secure registration and login"

# Backend Worktree
cd ../todomvc-backend
# Claude Code Session 2: "Design and implement REST API endpoints for todo operations"

# UI Worktree
cd ../todomvc-ui
# Claude Code Session 3: "Enhance UI with accessibility improvements and keyboard navigation"
```

### Phase 2: Integration Planning (Week 2)
```bash
# Create integration branch for testing feature combinations
cd todomvc-main
git checkout -b integration/all-features
git merge feature/authentication
git merge feature/backend-api
git merge feature/advanced-ui

# Test integration
npm install
npm run test
npm run serve
```

### Phase 3: Cross-Feature Coordination
```bash
# Authentication affects backend API
cd ../todomvc-auth
# "Implement JWT token passing to backend APIs"

cd ../todomvc-backend
# "Add authentication middleware to protect todo endpoints"

cd ../todomvc-ui
# "Update UI to handle authenticated vs anonymous states"
```

## Advanced Coordination Patterns

### 1. Shared Context Updates
Update `.claude/CLAUDE.md` files to reference other worktrees:
```markdown
# Authentication Context
This worktree focuses on user authentication.

## Related Work
- Backend API endpoints: `../todomvc-backend/src/api/`
- UI authentication states: `../todomvc-ui/src/components/auth/`

## Integration Points
- JWT tokens must be compatible with backend expectations
- UI login forms should match authentication flow
```

### 2. Cross-Worktree Testing
```bash
# Create test coordination script
#!/bin/bash
# test-all-features.sh

echo "Testing Authentication..."
cd ../todomvc-auth && npm run test

echo "Testing Backend..."
cd ../todomvc-backend && npm run test

echo "Testing UI..."
cd ../todomvc-ui && npm run test

echo "Testing Integration..."
cd ../todomvc-main && npm run test:integration
```

### 3. Progressive Integration
```bash
# Week 1: Individual feature testing
# Week 2: Pairwise integration (auth + backend, backend + ui, etc.)
# Week 3: Full integration testing
# Week 4: Performance and production readiness
```

## Benefits of This Approach

### Development Velocity
- **Parallel development** eliminates blocking dependencies
- **Specialized agents** provide focused expertise per domain
- **Independent testing** catches issues early per feature

### Code Quality
- **Specialized review** by domain-expert agents
- **Focused context** prevents feature creep and scope drift
- **Incremental integration** reduces merge conflicts

### Risk Management
- **Isolated experimentation** - risky changes don't affect other work
- **Easy rollback** - abandon problematic features without impact
- **Staged integration** - test feature combinations systematically

## Cleanup and Deployment
```bash
# After successful integration
git checkout main
git merge integration/all-features

# Clean up worktrees
git worktree remove ../todomvc-auth
git worktree remove ../todomvc-backend
git worktree remove ../todomvc-ui

# Deploy integrated solution
npm run build
npm run deploy
```

This approach scales from simple feature development to complex, multi-team enterprise workflows.