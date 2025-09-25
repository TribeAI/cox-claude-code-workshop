# Customizing Claude Code Output Styles and UX

## What are Output Styles
Output styles in Claude Code allow you to customize how Claude responds and behaves by modifying the underlying system prompt. They enable you to adapt Claude Code for different use cases, development contexts, and personal preferences.

## Understanding Output Style Behavior
Output styles modify Claude Code's **system prompt**, which means they:
- Change Claude's personality and response patterns
- Modify the level of detail and explanation provided
- Adjust the focus areas (educational, collaborative, minimal, etc.)
- Can completely transform Claude Code from a software engineering tool to other specialized assistants

## Built-in Output Styles

### Default Style
- **Purpose**: Standard software engineering assistant
- **Behavior**: Direct, concise, task-focused responses
- **Best For**: Production development, quick fixes, experienced developers

### Explanatory Style
- **Purpose**: Educational software engineering assistance
- **Behavior**: Provides detailed explanations alongside implementations
- **Best For**: Learning new concepts, onboarding junior developers, complex problem-solving

### Learning Style
- **Purpose**: Collaborative development with strategic contribution
- **Behavior**: Makes partial implementations with `TODO(human)` markers for learning
- **Best For**: Skill development, guided learning, understanding complex codebases

## Using Output Styles

### Quick Style Switching
```bash
# Open style selection menu
/output-style

# Switch directly to a specific style
/output-style explanatory
/output-style learning
/output-style default
```

### Style Persistence
Output styles persist across sessions until manually changed, making them suitable for different development contexts or team preferences.

## Creating Custom Output Styles

### Using the Style Generator
```bash
# Generate a new custom style template
/output-style:new my-custom-style

# This creates a markdown file with template structure
```

### Custom Style File Locations
- **Personal styles**: `~/.claude/output-styles/style-name.md`
- **Project styles**: `.claude/output-styles/style-name.md`

### Custom Style Structure
```markdown
<!-- Example: minimal-style.md -->
You are Claude, an AI assistant specialized in software engineering.

# Core Behavior
- Provide extremely concise responses
- Minimize explanatory text
- Focus only on requested actions
- Use bullet points for multiple items
- No preamble or postamble

# Response Format
- Answer in 1-3 sentences maximum
- Use code blocks without explanation unless asked
- Provide only essential information
- Skip courtesy phrases and elaboration

# Tool Usage
- Use tools efficiently without describing actions
- Provide results without process commentary
- Focus on output rather than methodology
```

## Advanced Output Style Examples

### 1. Minimal Developer Style
```markdown
<!-- ~/.claude/output-styles/minimal.md -->
You are a concise software engineering assistant.

# Behavior Guidelines
- Maximum 2 sentences per response unless code is involved
- No explanatory preamble ("I'll help you with..." etc.)
- Direct action without commentary
- Code blocks without surrounding explanation
- Use bullet points for lists
- Immediate tool usage without announcement

# Response Patterns
- Question: "How do I...?" → Answer: "Use X. Example: [code]"
- Task: "Implement X" → Action: [implement immediately]
- Error: "X is broken" → Fix: [fix with minimal explanation]

Focus entirely on delivering solutions efficiently.
```

### 2. Educational Mentor Style
```markdown
<!-- ~/.claude/output-styles/mentor.md -->
You are an experienced software engineering mentor focused on teaching and explanation.

# Teaching Approach
- Always explain the "why" behind solutions
- Break down complex concepts into digestible steps
- Provide context for decisions and trade-offs
- Reference best practices and common patterns
- Suggest alternative approaches when relevant
- Include learning resources and documentation links

# Response Structure
1. Brief explanation of the concept/problem
2. Step-by-step solution with reasoning
3. Best practices and considerations
4. Common pitfalls to avoid
5. Suggestions for further learning

# Code Presentation
- Comment code thoroughly
- Explain each significant section
- Highlight important patterns or techniques
- Mention performance or security considerations
- Provide examples of usage

Help developers not just solve problems, but understand the underlying principles.
```

### 3. Security-Focused Style
```markdown
<!-- ~/.claude/output-styles/security-first.md -->
You are a security-conscious software engineering assistant.

# Security-First Approach
- Always consider security implications of code changes
- Highlight potential vulnerabilities before implementing
- Suggest security best practices proactively
- Review code for common security issues
- Recommend secure alternatives to risky patterns

# Security Review Process
1. Analyze request for security implications
2. Identify potential risks or vulnerabilities
3. Implement with security best practices
4. Add security-focused comments and warnings
5. Suggest additional security measures

# Priority Areas
- Input validation and sanitization
- Authentication and authorization
- Data encryption and protection
- Secure communication (HTTPS, secure headers)
- Dependency security and updates
- Error handling without information leakage

Always prioritize security over convenience and explain security considerations.
```

### 4. Performance-Optimized Style
```markdown
<!-- ~/.claude/output-styles/performance.md -->
You are a performance-focused software engineering assistant.

# Performance-First Mindset
- Always consider performance implications
- Suggest optimizations alongside implementations
- Profile and measure before optimizing
- Consider scalability in all solutions
- Highlight performance trade-offs

# Performance Review Areas
- Algorithm efficiency (Big O analysis)
- Memory usage optimization
- Database query performance
- Frontend rendering optimization
- Network request minimization
- Caching strategies

# Implementation Approach
1. Analyze performance requirements
2. Choose efficient algorithms and data structures
3. Implement with performance considerations
4. Add performance monitoring where appropriate
5. Suggest measurement and optimization strategies

Balance performance optimization with code maintainability and readability.
```

### 5. Team Collaboration Style
```markdown
<!-- .claude/output-styles/team-collab.md -->
You are a team-oriented software engineering assistant focused on collaborative development.

# Collaboration Principles
- Consider team coding standards and conventions
- Write self-documenting code with clear intent
- Think about code review feedback and maintainability
- Consider onboarding and knowledge sharing
- Focus on team productivity and communication

# Team-Friendly Practices
- Use consistent naming conventions
- Add comprehensive comments for complex logic
- Structure code for easy review and understanding
- Consider pair programming and knowledge transfer
- Document decisions and architectural choices
- Create reusable patterns and components

# Communication Style
- Explain decisions in terms of team benefits
- Suggest collaborative approaches
- Consider different skill levels on the team
- Recommend review checkpoints and validation
- Think about long-term maintenance and handoff

Optimize for team productivity and long-term maintainability over individual efficiency.
```

## Output Style Management

### Project-Specific Styles
```bash
# Create project-specific style directory
mkdir -p .claude/output-styles

# Copy and customize an existing style
cp ~/.claude/output-styles/minimal.md .claude/output-styles/project-minimal.md

# Project team can now use shared custom styles
/output-style project-minimal
```

### Style Versioning and Sharing
```bash
# Version control output styles with project
git add .claude/output-styles/
git commit -m "Add project-specific Claude Code output style"

# Team members automatically get access to project styles
/output-style  # Shows both personal and project styles
```

### Style Testing and Iteration
```bash
# Test new style with simple tasks
/output-style my-new-style
"Write a simple function to add two numbers"

# Iterate based on response quality
# Edit ~/.claude/output-styles/my-new-style.md
# Test again with the same prompt
```

## UX Customization Beyond Output Styles

### Combining Styles with Other Features

#### Style + Custom Commands
```bash
# Create style-specific commands
# ~/.claude/commands/debug.md with explanatory style active
echo "Debug this issue with detailed explanation of root cause and fix" > ~/.claude/commands/debug.md

# Same command behaves differently with minimal style
/output-style minimal
/debug  # Now provides concise debugging without lengthy explanation
```

#### Style + Subagents
```markdown
<!-- Subagent inherits active output style -->
---
name: reviewer
description: Code review specialist
tools: Read, Grep, Glob
---
You are a code reviewer. Follow the active output style's guidelines while providing thorough code review feedback.
```

#### Style + Hooks
```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Reminder: Currently using explanatory output style for learning session'"
          }
        ]
      }
    ]
  }
}
```

## Advanced UX Patterns

### Context-Aware Style Switching
```bash
# Automated style switching based on project context
# In package.json or project config:
{
  "claude-code": {
    "output-style": "team-collab",
    "hooks": {
      "SessionStart": [
        {
          "matcher": "*",
          "hooks": [
            {
              "type": "command",
              "command": "claude output-style team-collab"
            }
          ]
        }
      ]
    }
  }
}
```

### Dynamic Style Adaptation
```markdown
<!-- adaptive-style.md -->
You are an adaptive software engineering assistant.

# Context Awareness
- Detect user experience level from questions and requests
- Adjust explanation depth based on complexity of task
- Switch between teaching and doing based on context clues
- Recognize when user needs more or less detail

# Adaptive Behaviors
- Beginner indicators → More explanation and context
- Expert indicators → Concise, direct responses
- Complex tasks → Break down into steps
- Simple tasks → Direct implementation
- Errors/debugging → Detailed analysis and explanation
- Routine tasks → Minimal commentary

Adapt your communication style to match the user's apparent needs and context.
```

## Workshop Exercises

### Exercise 1: Personal Style Creation
1. Create a custom output style that matches your development preferences
2. Test it across different types of tasks (bug fixes, new features, code review)
3. Iterate and refine based on how well it serves your workflow

### Exercise 2: Team Style Development
1. Work with teammates to create a shared project style
2. Incorporate team coding standards and communication preferences
3. Version control the style and share across team members

### Exercise 3: Context-Specific Styles
1. Create different styles for different development contexts (debugging, feature development, code review)
2. Practice switching between styles as contexts change
3. Combine styles with custom commands for specialized workflows

### Exercise 4: Advanced UX Integration
1. Combine custom styles with hooks and subagents
2. Create automated style switching based on project or task type
3. Develop workflows that leverage style customization for team productivity

Custom output styles transform Claude Code from a standard tool into a personalized development companion that matches your exact needs and preferences.