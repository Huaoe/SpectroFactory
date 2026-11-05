# AGENTS.md

## Required workflow

- Always use `graft` whenever invoking or coordinating an agent or sub-agent in this repository.
- Do not run agent tasks directly without routing them through `graft`.
- Treat `graft` as the default execution path for any agent-driven workflow in this project.

## Example

- Use `graft` for agent orchestration, delegation, and task execution.
- If a workflow requires a direct agent call, prefer the `graft` wrapper or equivalent `graft`-managed runner.

## Purpose

This rule keeps agent execution consistent, trackable, and aligned with the project’s tooling.
