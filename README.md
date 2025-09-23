# Claude Code Hands-On Workshop

## Purpose

This hands-on workshop is designed to **showcase intermediate-to-advanced features of Claude Code** through structured, hands-on tracks. Participants will get to see end-to-end development cycles automated by Claude, leveraging **subagents** and **MCP servers** for documentation and automated testing.

By the end, attendees should understand how to:

- Apply **PRDs (Product Requirements Documents)** to an existing repo with minimal human intervention.
- Use **subagents** to generate structured task plans and execute them.
- Integrate **MCP servers** for documentation access, browser testing, and visual validation.
- Run a full loop: spec → code → tests → PR.

---

## Logistics

- **Duration**: 1.5 hours total (flexible)
- **Participants**: Small groups (2–4 per table). Works well for 20–40 attendees.
- **Pre-reqs**:
    - Laptops with GitHub access.
    - Node.js + npm installed.
    - Git CLI and `gh` (GitHub CLI) installed and authenticated.
    - Claude Code setup in their IDE/editor with MCP enabled.
- **Repos**: Fork [TodoMVC React](https://github.com/tastejs/todomvc/tree/gh-pages/examples/react).

---

## Track 1: SDLC Hands-On Build (TODO App)

### Goal

Demonstrate a **fully automated end-to-end dev cycle** where Claude Code executes against a well-specified PRD.

### Resources

- **PRDs**: 
    - See example PRD markdown files [here]()
- **MCP Servers**:
    - [Playwright MCP](https://github.com/microsoft/playwright-mcp?tab=readme-ov-file#getting-started)
    - [Context7 MCP](https://github.com/upstash/context7?tab=readme-ov-file#%EF%B8%8F-installation)
- **Subagents**:
    - 
    - See example agent markdown files [here]()
    - Alternatively, try creating a new agent (following the formatting guidelines in the [Claude docs](https://docs.claude.com/en/docs/claude-code/sub-agents#file-format))

### Getting Started with the TodoMVC React Repo

1. **Clone the Repo**
    
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
    - Add your chosen `PRD-*.md` file to this folder.
5. **Start Claude Code**
    - Launch Claude Code in your IDE.
    - Ensure any MCP servers are configured (Playwright MCP, Context7, etc.) as needed .
    - Follow the guide/example for creating a [`CLAUDE.md`](http://CLAUDE.md) file.

### Steps

### 1. Engineer Setup

1. Pick one of the provided PRDs.
2. Copy it into the repo root.
3. Add MCP servers to Claude Code.
4. Start Claude Code in **YOLO mode**.
5. Prompt Claude Code to spin up a subagent and generate a **task list** based on the PRD.

### 2. Claude Code Execution

- **Read Docs** (`CLAUDE.md`, repo docs).
- **Implement Feature**.
- **Unit + Browser Tests** (Playwright MCP).
- **Create PR** via GitHub CLI.

### 3. Engineer Review

- Review PR in GitHub (or chosen tool).
- Merge PR.
- Review the updates in the app running locally.

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

### Facilitator Notes

- Encourage **incremental scope** — BYOC can spiral if the repo is large. Recommend tasks that are **self-contained (1–2 files)**.
- Have participants add a `CLAUDE.md` to their repo (you can repurpose the one provided).

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