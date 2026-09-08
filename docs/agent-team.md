# Agent team for Mona's Project Pulse dashboard

I am using GitHub Copilot CLI in a Codespace to orchestrate the work and coordinate a small custom agent team for the Project Pulse dashboard build.

## Custom agents

### Planner
- Target model: Claude Opus 4.7 (copilot)
- Responsibility: Research the repository, review docs and dependencies, identify edge cases, and produce a practical implementation plan with file ownership, sequencing, validation expectations, and open questions.
- Definition: `.github/agents/planner.agent.md`

### Orchestrator
- Target model: Claude Opus 4.7 (copilot)
- Responsibility: Break the work into phases, delegate tasks to specialist agents, keep dependencies and file scopes organized, and verify the final integration before reporting progress.
- Definition: `.github/agents/orchestrator.agent.md`

### Designer
- Target model: Gemini 3.1 Pro (copilot)
- Responsibility: Drive the dashboard UX and visual design, including information hierarchy, accessibility, interaction flow, responsive layout, and styling for a polished Project Pulse interface.
- Definition: `.github/agents/designer.agent.md`

### Coder
- Target model: GPT-5.5 (copilot)
- Responsibility: Implement the code changes, fix logic issues, build the runnable app scaffolding when needed, and validate behavior within the file scope assigned by the Orchestrator.
- Definition: `.github/agents/coder.agent.md`

This team gives the project a clear division of labor: the Planner defines what to build, the Orchestrator coordinates delivery, the Designer shapes the experience, and the Coder turns the approved plan into working code.
