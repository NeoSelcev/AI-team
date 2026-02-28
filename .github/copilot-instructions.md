## Project: Copilot Orchestra

This is a multi-agent development team for GitHub Copilot. The workspace contains agent definitions in `.github/agents/`.

### Conventions

- Agent files use kebab-case filenames: `agent-name.agent.md`
- Agent files live in `.github/agents/`
- All subagents are hidden from the user picker (`user-invocable: false`) — only the team lead is user-facing
- Agents communicate routing suggestions using the format: `ROUTE_SUGGESTION: <agent> — <reason>`

### Workflow

The team lead follows this sequence:
1. Research (researcher) → Plan (planner) → User approval
2. For each unit: Implement (implementer) → Test (qa) → Review (reviewer)
3. As needed: Architecture input (architect), security review (security-auditor), debugging (debugger)
4. Mandatory stop after each unit for user commit
5. Final test suite run → documentation (documenter) → completion

### Plan Files

Plans are written to `plans/<task-name>.md` as a single file updated in place. Status values: `PENDING`, `IN PROGRESS`, `IN REVIEW`, `DONE`, `BLOCKED`.

### Code Style

- Follow TDD: tests first (expect failure), minimal code to pass, verify green, then lint/format
- Git commits follow conventional commits: `fix/feat/chore/test/refactor: description`
