# Copilot Orchestra

A multi-agent development team for GitHub Copilot. Instead of one agent doing everything, a **team lead** leads a team of 9 specialist agents — each with a focused role, clear boundaries, and the ability to route work to teammates.

## How It Works

Select the **team-lead** from the agent picker in VS Code chat. Describe your task. The team lead will:

1. Send the **researcher** to explore the codebase
2. Have the **planner** break down the work
3. Loop through implementation units: **implementer** → **qa** → **reviewer**
4. Bring in the **architect**, **security-auditor**, or **debugger** when needed
5. Stop at every milestone for your approval and commit

Agents communicate through the team lead using `ROUTE_SUGGESTION:` — if the reviewer thinks the design is wrong, it suggests routing to the architect. The team lead presents the suggestion to you and you decide.

## The Team

| Agent | Role | Model |
|-------|------|-------|
| **team-lead** | Leads the team, routes work, tracks progress | Opus 4.6 |
| **planner** | Breaks tasks into structured, actionable plans | Sonnet 4.6 |
| **researcher** | Explores codebase, gathers facts, reads docs | Sonnet 4.6 |
| **architect** | Design decisions, system structure, API design | Opus 4.6 |
| **implementer** | Writes code following TDD principles | Sonnet 4.6 |
| **qa** | Writes tests, runs suites, checks coverage | Haiku 4.5 |
| **reviewer** | Reviews changes for quality and correctness | Sonnet 4.6 |
| **security-auditor** | Audits for OWASP Top 10 vulnerabilities | Opus 4.6 |
| **debugger** | Diagnoses failures, traces bugs, finds root causes | Sonnet 4.6 |
| **documenter** | Writes READMEs, API docs, changelogs | Haiku 4.5 |

Only the team lead appears in the agent picker. All others are subagents invoked automatically.

## Key Features

- **Dynamic routing** — any agent can suggest rerouting work to a more appropriate teammate
- **Loop detection** — if work bounces between agents without progress, the team lead stops and asks you
- **Single plan file** — one `plans/<task-name>.md` file updated in place as work progresses
- **Mandatory checkpoints** — the team lead pauses after planning and after each unit for your approval
- **Clear boundaries** — each agent knows what it does and what it does NOT do

## Setup

1. Clone this repo (or download it) into your project as `.github/agents/` and `.github/copilot-instructions.md`
2. Open your project in VS Code with GitHub Copilot
3. In the chat panel, select **team-lead** from the agent picker
4. Describe your task

> **Tip:** Edit `.github/copilot-instructions.md` to add your project-specific conventions (language, framework, testing tools, etc.)

## File Structure

```
.github/
  agents/
    team-lead.agent.md          ← the team lead (user-facing)
    planner.agent.md            ← subagent
    researcher.agent.md         ← subagent
    architect.agent.md          ← subagent
    implementer.agent.md        ← subagent
    qa.agent.md                 ← subagent
    reviewer.agent.md           ← subagent
    security-auditor.agent.md   ← subagent
    debugger.agent.md           ← subagent
    documenter.agent.md         ← subagent
  copilot-instructions.md       ← workspace-wide instructions
README.md
```

## Model Configuration

Agents ship with Anthropic Claude defaults. The models are assigned in three tiers based on the reasoning depth each role needs:

| Tier | Role | Default (Anthropic) | OpenAI Equivalent | Google Equivalent |
|------|------|--------------------|--------------------|--------------------|
| **1 — Deep reasoning** | team-lead, architect, security-auditor | Claude Opus 4.6 | GPT-5.2 Thinking | Gemini 3.1 Pro |
| **2 — Analysis** | planner, researcher, reviewer, debugger, implementer | Claude Sonnet 4.6 | GPT-5.2 | Gemini 3 Pro |
| **3 — Fast execution** | qa, documenter | Claude Haiku 4.5 | GPT-5 Mini / Nano | Gemini 3 Flash |

To switch providers, edit the `model:` field in each agent's YAML frontmatter. The format is `Model Name (copilot)` — for example, `GPT-5.2 (copilot)` or `Gemini 3 Pro (copilot)`.

## Customization

- **Unhide a subagent**: Change `user-invocable: false` to `user-invocable: true` in that agent's frontmatter
- **Change models**: Edit the `model:` field in any agent's frontmatter (see [Model Configuration](#model-configuration) for tier guidance)
- **Add MCP tools**: Add MCP server references to an agent's `tools:` list (e.g., `'myserver/*'`)
- **Add project-specific instructions**: Edit `.github/copilot-instructions.md`
