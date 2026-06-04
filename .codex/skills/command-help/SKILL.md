---
name: command-help
description: Recommend the right Codex skill, agent, workflow, or effort level for a user request when they ask for help choosing how to proceed.
---

# Command Help

Use this skill when the user asks what command/workflow/skill to use, asks for current status, or asks how much reasoning effort is appropriate.

## Workflow

1. Classify the request: chat, scout, plan, implement, test, review, docs, git, design, database, or MCP.
2. Recommend the smallest Codex-native skill or agent that fits.
3. Include effort guidance only when useful.

## Effort Mapping

- `1` = `low`
- `2` = `medium`
- `3` = `high`
- `4` = `xhigh`

## Rules

- Do not recommend legacy slash commands.
- Prefer skills over raw commands when a skill exists.
- Keep recommendations short and actionable.
