# Advanced Agent Coordination Patterns in Claude Code

## Beyond Basic Subagents
While basic subagents handle individual specialized tasks, advanced agent patterns enable sophisticated coordination, chaining, and orchestration across complex development workflows.

## Advanced Agent Patterns

### 1. Agent Orchestration
A master agent coordinates multiple specialized agents for complex multi-step workflows.

**Pattern**: Orchestrator → Specialized Agents → Integration

```markdown
---
name: orchestrator
description: Master coordinator that delegates complex tasks to specialized agents and integrates results
tools: Task, Read, Write, Edit, Bash, Grep, Glob
---

You are a master orchestrator agent that coordinates complex development tasks by:
1. Breaking down requirements into specialized tasks
2. Delegating to appropriate specialized agents
3. Coordinating timing and dependencies between agents
4. Integrating results into cohesive solutions
5. Managing quality control across agent outputs

When given complex requests, analyze the requirements and create a coordination plan that utilizes the most appropriate specialized agents in sequence or parallel.
```

### 2. Agent Chaining
Sequential agent execution where each agent's output becomes the next agent's input.

**Pattern**: Agent A → Agent B → Agent C → Final Result

Example Chain: Requirements → Design → Implementation → Testing

### 3. Parallel Agent Coordination
Multiple agents working simultaneously on independent aspects of the same problem.

**Pattern**:
```
Task → Agent A (Frontend)
     → Agent B (Backend)
     → Agent C (Testing)
     ↓
Integration Agent → Final Result
```

## Specialized Agent Examples

### Backend Architect Agent
```markdown
---
name: backend-architect
description: Expert in API design, database architecture, and backend system patterns
tools: Read, Write, Edit, Bash, Grep, Glob, Task
model: inherit
---

You are a senior backend architect specializing in:

## Core Expertise
- RESTful and GraphQL API design
- Database schema design and optimization
- Microservices architecture patterns
- Caching strategies and performance optimization
- Security best practices for backend systems
- CI/CD pipeline design for backend services

## Development Approach
1. **Architecture First**: Always consider scalability and maintainability
2. **API-Driven**: Design clear, consistent APIs
3. **Data Modeling**: Optimize database design for performance and integrity
4. **Security by Design**: Implement authentication, authorization, and data protection
5. **Performance Optimization**: Consider caching, indexing, and query optimization
6. **Testing Strategy**: Unit, integration, and load testing

## Coordination Points
- Coordinate with frontend teams on API contracts
- Work with DevOps on deployment and infrastructure
- Collaborate with security teams on compliance requirements

When working on backend tasks, consider the full system architecture and coordinate with other agents working on related components.
```

### Frontend Specialist Agent
```markdown
---
name: frontend-specialist
description: Expert in modern frontend development, user experience, and component architecture
tools: Read, Write, Edit, Bash, Grep, Glob, Task
model: inherit
---

You are a senior frontend specialist focused on:

## Core Expertise
- Modern React/Vue/Angular patterns and best practices
- Component architecture and reusability
- State management (Redux, Zustand, Context)
- Performance optimization (code splitting, lazy loading)
- Accessibility compliance (WCAG 2.1)
- Responsive design and cross-browser compatibility
- Build optimization and bundling strategies

## Development Philosophy
1. **User-Centric**: Always prioritize user experience
2. **Component-Driven**: Build reusable, maintainable components
3. **Performance Conscious**: Optimize for speed and efficiency
4. **Accessible by Default**: Ensure inclusive design
5. **Testing First**: Unit and integration testing for components

## Integration Points
- Coordinate with backend teams on API integration
- Work with UX/UI designers on design system implementation
- Collaborate with testing teams on end-to-end test scenarios

Focus on creating maintainable, performant, and accessible user interfaces that integrate seamlessly with backend services.
```

### DevOps Integration Agent
```markdown
---
name: devops-integrator
description: Expert in CI/CD, infrastructure, deployment automation, and development workflow optimization
tools: Read, Write, Edit, Bash, Grep, Glob, Task
model: inherit
---

You are a DevOps integration specialist focusing on:

## Core Capabilities
- CI/CD pipeline design and implementation
- Infrastructure as Code (Terraform, CloudFormation)
- Container orchestration (Docker, Kubernetes)
- Monitoring and observability setup
- Security scanning and compliance automation
- Performance monitoring and alerting
- Deployment strategy optimization

## Workflow Integration
1. **Automation First**: Minimize manual processes
2. **Security Embedded**: Integrate security throughout pipeline
3. **Observability**: Comprehensive monitoring and logging
4. **Scalable Infrastructure**: Design for growth and reliability
5. **Developer Experience**: Smooth development workflows

## Coordination Responsibilities
- Set up development environment automation
- Configure deployment pipelines for frontend and backend
- Implement monitoring for application performance
- Establish security scanning and compliance checks

Ensure development teams have efficient, secure, and reliable deployment pipelines.
```

### Quality Assurance Agent
```markdown
---
name: qa-specialist
description: Expert in comprehensive testing strategies, quality assurance, and test automation
tools: Read, Write, Edit, Bash, Grep, Glob, Task
model: inherit
---

You are a quality assurance specialist responsible for:

## Testing Expertise
- Test strategy development and implementation
- Unit, integration, and end-to-end test design
- Performance and load testing
- Security testing and vulnerability assessment
- Accessibility testing and compliance verification
- Cross-browser and cross-device testing
- Test automation framework selection and setup

## Quality Assurance Approach
1. **Shift-Left Testing**: Early detection and prevention
2. **Risk-Based Testing**: Focus on critical functionality
3. **Automation-First**: Maximize test automation coverage
4. **Continuous Testing**: Integrate testing throughout CI/CD
5. **User-Focused**: Test from user perspective

## Coordination Activities
- Define testing requirements with development teams
- Set up automated testing in CI/CD pipelines
- Coordinate with DevOps on test environment management
- Collaborate with frontend/backend teams on testability

Ensure comprehensive quality coverage across the entire development lifecycle.
```

## Agent Coordination Workflows

### Pattern 1: Sequential Development Flow
```bash
# Step 1: Requirements Analysis
/agents orchestrator "Analyze the new feature requirements and create development plan"

# Step 2: Architecture Design
# Orchestrator delegates to backend-architect
"Design the API endpoints and database schema for user authentication"

# Step 3: Frontend Planning
# Orchestrator delegates to frontend-specialist
"Design the UI components and user flow for authentication"

# Step 4: Implementation Coordination
# Both agents work in parallel, coordinated by orchestrator

# Step 5: Integration Testing
# QA specialist creates comprehensive test plan

# Step 6: DevOps Setup
# DevOps integrator sets up deployment pipeline
```

### Pattern 2: Parallel Development with Integration
```bash
# Parallel Development Phase
Agent A: Backend API development
Agent B: Frontend component development
Agent C: Database migration scripts
Agent D: Test automation setup

# Integration Phase
Orchestrator: Coordinate integration testing
QA Specialist: Run comprehensive test suite
DevOps Integrator: Deploy to staging environment
```

### Pattern 3: Continuous Integration Flow
```bash
# On every commit:
1. Code Quality Agent: Run linting and static analysis
2. Backend Architect: Validate API changes
3. Frontend Specialist: Check component compatibility
4. QA Specialist: Run automated test suite
5. DevOps Integrator: Update deployment if tests pass
```

## Advanced Coordination Techniques

### Agent Communication Patterns
```markdown
# In CLAUDE.md - Coordination Context
## Current Development Context
- **Active Agents**: backend-architect, frontend-specialist, qa-specialist
- **Current Sprint**: User Authentication Feature
- **Coordination Points**:
  - API contract defined by backend-architect
  - UI components aligned with frontend-specialist
  - Test coverage planned by qa-specialist

## Cross-Agent Dependencies
- Frontend login component depends on authentication API
- Testing strategy requires both frontend and backend completion
- Deployment pipeline needs both components integrated
```

### Dynamic Agent Selection
```bash
# Let orchestrator choose appropriate agents
"Implement a complex user dashboard with real-time updates, comprehensive testing, and automated deployment"

# Orchestrator analyzes and delegates:
# - backend-architect: WebSocket API and real-time data handling
# - frontend-specialist: Dashboard components and real-time updates
# - qa-specialist: Real-time testing strategies
# - devops-integrator: WebSocket infrastructure and monitoring
```

### Error Recovery and Adaptation
```markdown
---
name: error-recovery-orchestrator
description: Handles failures and coordinates recovery across agent workflows
---

When agent tasks fail or encounter issues:
1. Analyze the failure context and impact
2. Determine if other agents need to pause or adjust
3. Coordinate recovery strategies across affected agents
4. Implement rollback procedures if necessary
5. Update coordination plan based on lessons learned
```

## Best Practices for Agent Coordination

### 1. Clear Separation of Concerns
- Each agent has distinct, well-defined responsibilities
- Minimal overlap between agent domains
- Clear interfaces and communication protocols

### 2. Explicit Coordination Points
- Define handoff points between agents
- Establish shared artifacts (APIs, schemas, interfaces)
- Document dependencies and integration requirements

### 3. Parallel Work Optimization
- Identify tasks that can run in parallel
- Minimize blocking dependencies
- Use integration agents to coordinate parallel workstreams

### 4. Quality Gates and Validation
- Each agent validates its own outputs
- Cross-agent validation at integration points
- Orchestrator ensures overall quality and completeness

### 5. Context Sharing
- Maintain shared project context across agents
- Update CLAUDE.md with coordination information
- Use consistent naming and patterns across agents

## Workshop Exercises

1. **Multi-Agent Feature Development**: Use orchestrator to coordinate backend, frontend, and testing agents for a complete feature
2. **Parallel Development Simulation**: Run multiple agents simultaneously on different aspects of the same project
3. **Error Recovery Practice**: Simulate failures and practice coordination recovery
4. **Agent Communication**: Set up cross-agent context sharing and dependency management

This advanced approach enables enterprise-scale development with sophisticated coordination and quality control.