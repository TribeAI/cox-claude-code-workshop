# Claude Code Hands-On Workshop

## Purpose

This hands-on workshop is designed to **showcase intermediate-to-advanced features of Claude Code** through structured, hands-on tracks. Participants will master basic workflows AND genuinely advanced patterns like parallel development, production automation, and multi-agent orchestration.

By the end, attendees should understand how to:

- Apply **PRDs (Product Requirements Documents)** to an existing repo with minimal human intervention.
- Use **subagents** to generate structured task plans and execute them.
- Integrate **MCP servers** for documentation access, browser testing, and visual validation.
- Run a full loop: spec → code → tests → PR.
- **Master advanced workflows**: parallel development with worktrees, production-ready hooks, and multi-agent coordination.

---

## Logistics

- **Duration**: 1.5 hours total (flexible)
- **Pre-reqs**:
    - Laptops with GitHub access.
    - Node.js + npm installed (if choosing track 1).
    - Git CLI and `gh` (GitHub CLI) installed and authenticated.
    - Claude Code setup in their IDE/editor with MCP enabled.
- **Repos**: Fork [TodoMVC React](https://github.com/tastejs/todomvc/tree/gh-pages/examples/react) (if choosing track 1).

---

## Track 1: SDLC Hands-On Build (TODO App)

### Goal

Demonstrate a **fully automated end-to-end dev cycle** where Claude Code executes against a well-specified PRD.

### Resources

- **PRD Examples**: 
    - See example PRD markdown files [here](https://github.com/TribeAI/cox-claude-code-workshop/tree/main/examples/PRDs)
- **Subagents Examples**:
    - See example agent markdown files [here](https://github.com/TribeAI/cox-claude-code-workshop/tree/main/examples/agents)
    - Alternatively, try creating a new agent (following the formatting guidelines in the [Claude docs](https://docs.claude.com/en/docs/claude-code/sub-agents#file-format))
- **Custom Slash Command Examples**:
    - See example custom slash commands [here](https://github.com/TribeAI/cox-claude-code-workshop/tree/main/examples/commands) 
- **MCP Server Docs**:
    - [Playwright MCP](https://github.com/microsoft/playwright-mcp?tab=readme-ov-file#getting-started)
    - [Context7 MCP](https://github.com/upstash/context7?tab=readme-ov-file#%EF%B8%8F-installation)
- **Advanced Guides**:
    - See [`guides/`](./guides/) for focused guides on worktrees and other advanced patterns
    - All practical configurations available in [`examples/`](./examples/)

### Getting Started with the TodoMVC React Repo

1. **Clone the [ToDo Repo](https://github.com/tastejs/todomvc/tree/master)**
    
    ```bash
    git clone https://github.com/tastejs/todomvc.git
    cd todomvc/examples/react
    ```
    
2. **Install Dependencies**
    
    ```bash
    npm install
    ```
    
3. **Run the App**
    
    ```bash
    npm run serve
    ```
    
4. **Open in Your Editor**
    - Open the `examples/react` folder in VS Code (or your preferred IDE).
    - Add your chosen/created `PRD-*.md` and `agent-*.md` files to this folder.
5. **Start Claude Code**
    - Launch Claude Code in your IDE.
    - Ensure any MCP servers are configured (Playwright MCP, Context7, etc.) as needed .
    - Follow the guide for creating/updating a [`CLAUDE.md`](https://github.com/TribeAI/cox-claude-code-workshop/blob/main/guides/Applying_CLAUDE_md.md) file.

### Steps

### 1. Engineer Setup

1. Pick one of the provided PRD example markdown files and modify it (or create a new one from scratch) and copy it into the repo root (i.e., `examples/react/`)
2. Pick one of the provided subagent markdown files and modify it (or create a new one from scratch) and copy it into the repo (i.e., `examples/react/.claude/agents/`)
    - Alternatively (recommended) do this at a later stage via the interface by calling `/agents` after Claude Code has been initialized.
3. Add MCP servers to Claude Code (optional, as desired)
4. Start Claude Code in **YOLO mode** and add a `CLAUDE.md` file.
5. Prompt Claude Code to spin up a subagent and generate a **task list** based on the PRD.

### 2. Claude Code Execution

- **Read Docs** (`CLAUDE.md`, repo docs).
- **Implement Feature**.
- **Unit + Browser Tests** (e.g., via Playwright MCP).
- **Create PR** via GitHub CLI.

### 3. Engineer Review

- Review PR in GitHub (or chosen tool).
- Merge PR.
- Review the updates in the app running locally.

### 4. Repeat.

---

## Track 2: BYOC (Bring Your Own Code)

### Goal

Allow experienced developers to apply Claude Code concepts to **their own repositories** instead of the provided TODO app. This gives participants a chance to explore **subagents, MCP servers, and task automation** in a codebase that matters to them.

### Requirements

- Participants must have:
    - An active repo they’re comfortable experimenting with.
    - Tests set up (unit or integration).
    - GitHub CLI installed for PR creation.

### Suggested Prompts

Developers can try:

- **Spec-driven dev**:
    
    > “Here’s a new feature request: [paste PRD or Jira ticket]. Please use the generate-tasks subagent, follow CLAUDE.md conventions, and implement the feature with tests and a PR.”
    > 
- **Refactoring task**:
    
    > “Please refactor all class components to functional components with hooks, add tests to confirm unchanged behavior, and create a PR.”
    > 
- **Testing enhancement**:
    
    > “Add browser tests for user login using Playwright MCP. Make sure they run successfully before creating a PR.”
    > 

---

## Track 3: Advanced Problem-Solving Skills

### Goal
Master **advanced Claude Code problem-solving techniques** that dramatically improve your daily development workflow: debugging stuck conversations, managing context in large codebases, and sophisticated prompting for complex scenarios.

### Duration
25 minutes per section (pick 1-2 based on your current challenges)

### Prerequisites
- Completed Track 1 or 2, OR
- Existing Claude Code experience with basic workflows
- Experience with getting "stuck" or frustrated with Claude responses

### Section A: Parallel Development Mastery
**Skill**: Use git worktrees to develop multiple features simultaneously with focused Claude Code sessions

**Hands-On Exercise**:
1. Set up 2 worktrees for TodoMVC with parallel features (categories + search)
2. Run focused Claude Code sessions in each worktree
3. Develop features independently, then integrate
4. **Success Criteria**: Complete 2 features faster than sequential development

**Real Scenario**: "Add dark mode AND user preferences without context switching overhead"

### Section B: Debugging Stuck Workflows
**Skill**: Quickly recover when Claude gets confused, over-analyzes, or produces poor results

**Hands-On Exercise**:
1. Intentionally create a "stuck" scenario with vague requirements
2. Practice recovery techniques: reset & redirect, scope limiting, progressive disclosure
3. Transform confused conversation into productive implementation in under 5 minutes
4. **Success Criteria**: Master 3 recovery patterns applicable to your daily workflow

**Real Scenario**: "Claude is over-analyzing instead of implementing" → Working feature in minutes

### Section B: Advanced Context Management
**Skill**: Guide Claude effectively in large, complex codebases without information overload

**Hands-On Exercise**:
1. Take a complex feature request for TodoMVC (or your own codebase)
2. Practice context layering, selective file reading, and pattern recognition
3. Guide Claude to understand and modify code without getting lost
4. **Success Criteria**: Claude makes meaningful changes with no confusion or backtracking

**Real Scenario**: "Add authentication system" → Clean implementation that follows existing patterns

### Section C: Context Layering
**Skill**: Guide Claude effectively in large, complex codebases without information overload

**Hands-On Exercise**:
1. Take a complex feature request for TodoMVC (or your own codebase)
2. Practice context layering, selective file reading, and pattern recognition
3. Guide Claude to understand and modify code without getting lost
4. **Success Criteria**: Claude makes meaningful changes with no confusion or backtracking

**Real Scenario**: "Add authentication system" → Clean implementation that follows existing patterns

### Section D: Sophisticated Prompting Patterns
**Skill**: Craft advanced prompts that get superior results for complex, real-world scenarios

**Hands-On Exercise**:
1. Learn constraint-driven development, result-driven prompting, and multi-phase problem solving
2. Compare basic prompts vs advanced patterns on the same task
3. Practice production-mindset vs prototype-mindset prompting
4. **Success Criteria**: Advanced prompts produce dramatically better results than basic ones

**Real Scenario**: Transform "make it better" → Specific, measurable improvements with clear success criteria

### Resources
- **Parallel Development**: [`guides/Parallel_Development_with_Worktrees.md`](./guides/Parallel_Development_with_Worktrees.md) + [`examples/worktrees/`](./examples/worktrees/)
- **Problem-Solving**: [`examples/problem-solving/`](./examples/problem-solving/) - Copy-paste patterns for common scenarios
- **Production Automation**: [`examples/hooks/`](./examples/hooks/) - Quality gates and team integration

### Why These Are Genuinely Advanced
- **Parallel Development**: Worktree workflows that enable focused, simultaneous development
- **Problem-Solving Focus**: Skills you need daily but can't learn from basic documentation
- **Immediate Impact**: Techniques that improve your workflow from day one
- **Real Scenarios**: Based on actual frustrations developers face with Claude Code
- **Transferable Skills**: Patterns that work across any codebase or technology stack

---

## Stretch Goals (Optional Mini-Tracks)

- **Subagent Chain**: Planning → coding → testing.
- **Visualization MCP**: Hook Claude into a screenshot/preview MCP.
- **Repo-wide Refactor**: Try automated, large-scale changes.

---

## Wrap-Up

- Each group demos their PR(s).
- BYOC participants share wins + blockers.
- Highlight:
    - Strengths of Claude Code (task decomposition, test execution).
    - Where **human oversight** remains essential.
    - How this maps to **real enterprise workflows**.