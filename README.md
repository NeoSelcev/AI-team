# Copilot Orchestra

A multi-agent development team for GitHub Copilot. Instead of one agent doing everything, an **orchestrator** leads a team of 9 specialist agents — each with a focused role, clear boundaries, and the ability to route work to teammates.

## How It Works

Select the **orchestrator** from the agent picker in VS Code chat. Describe your task. The orchestrator will:

1. Send the **researcher** to explore the codebase
2. Have the **planner** break down the work
3. Loop through implementation units: **implementer** → **tester** → **reviewer**
4. Bring in the **architect**, **security-reviewer**, or **debugger** when needed
5. Stop at every milestone for your approval and commit

Agents communicate through the orchestrator using `ROUTE_SUGGESTION:` — if the reviewer thinks the design is wrong, it suggests routing to the architect. The orchestrator presents the suggestion to you and you decide.

## The Team

| Agent | Role | Model |
|-------|------|-------|
| **orchestrator** | Leads the team, routes work, tracks progress | Sonnet 4.5 |
| **planner** | Breaks tasks into structured, actionable plans | Sonnet 4.5 |
| **researcher** | Explores codebase, gathers facts, reads docs | Sonnet 4.5 |
| **architect** | Design decisions, system structure, API design | Sonnet 4.5 |
| **implementer** | Writes code following TDD principles | Haiku 4.5 |
| **tester** | Writes tests, runs suites, checks coverage | Haiku 4.5 |
| **reviewer** | Reviews changes for quality and correctness | Sonnet 4.5 |
| **security-reviewer** | Audits for OWASP Top 10 vulnerabilities | Sonnet 4.5 |
| **debugger** | Diagnoses failures, traces bugs, finds root causes | Sonnet 4.5 |
| **documenter** | Writes READMEs, API docs, changelogs | Haiku 4.5 |

Only the orchestrator appears in the agent picker. All others are subagents invoked automatically.

## Key Features

- **Dynamic routing** — any agent can suggest rerouting work to a more appropriate teammate
- **Loop detection** — if work bounces between agents without progress, the orchestrator stops and asks you
- **Single plan file** — one `plans/<task-name>.md` file updated in place as work progresses
- **Mandatory checkpoints** — the orchestrator pauses after planning and after each unit for your approval
- **Clear boundaries** — each agent knows what it does and what it does NOT do

## Setup

1. Clone this repo (or download it)
2. Copy the agent files and instructions into your project:

```bash
# From your project root:
mkdir -p .github/agents

# Copy all agents
cp /path/to/copilot-orchestra/*.agent.md .github/agents/
```

3. Open your project in VS Code with GitHub Copilot
4. In the chat panel, select **orchestrator** from the agent picker
5. Describe your task

> **Tip:** Edit `.github/copilot-instructions.md` to add your project-specific conventions (language, framework, testing tools, etc.)

## File Structure

This repo (template):
```
*.agent.md                  ← agent definitions
.github/
  copilot-instructions.md   ← workspace-wide instructions
README.md
```

Your project (after setup):
```
.github/
  agents/
    orchestrator.agent.md       ← the team lead (user-facing)
    planner.agent.md            ← subagent
    researcher.agent.md         ← subagent
    architect.agent.md          ← subagent
    implementer.agent.md        ← subagent
    tester.agent.md             ← subagent
    reviewer.agent.md           ← subagent
    security-reviewer.agent.md  ← subagent
    debugger.agent.md           ← subagent
    documenter.agent.md         ← subagent
  copilot-instructions.md       ← workspace-wide instructions
```

## Customization

- **Unhide a subagent**: Change `user-invocable: false` to `user-invocable: true` in that agent's frontmatter
- **Change models**: Edit the `model:` field in any agent's frontmatter
- **Add MCP tools**: Add MCP server references to an agent's `tools:` list (e.g., `'myserver/*'`)
- **Add project-specific instructions**: Edit `.github/copilot-instructions.md`
