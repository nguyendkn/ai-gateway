---
name: agent-builder
description: Create Codex custom agent .toml files from a reusable template. Use when the user asks to create, convert, scaffold, or standardize a Codex agent/subagent, especially from legacy agent markdown, AGENTS.md notes, role descriptions, or workflow instructions.
---

# Codex Agent Template Skill

Use this skill to create a valid Codex custom agent `.toml` file from a user request, pasted instructions, a legacy provider-style agent, or an existing workflow.

## Purpose

Produce one focused Codex custom agent that can be saved under:

- Project scope: `.codex/agents/<agent-name>.toml`
- User scope: `~/.codex/agents/<agent-name>.toml`

Codex custom agent files must include:

- `name`
- `description`
- `developer_instructions`

Optional fields may include:

- `model`
- `model_reasoning_effort`
- `sandbox_mode`
- `nickname_candidates`
- `mcp_servers`
- `skills.config`

## Inputs to infer

When the user does not provide every field, infer reasonable defaults instead of asking follow-up questions.

Infer:

1. `name`
   - Use kebab-case filename convention but TOML `name` should use snake_case when the agent is referenced by Codex.
   - Example filename: `git-manager.toml`
   - Example `name`: `git_manager`

2. `description`
   - One sentence.
   - Must clearly say when to use the agent.
   - Include trigger words near the beginning.

3. `model`
   - Default: omit unless the user explicitly requests a model.
   - If task is light/ops: use a smaller/faster model if configured by user.
   - If task requires review/security/architecture: use a stronger model if configured by user.
   - If `model` is specified, `model_reasoning_effort` must also be specified.

4. `model_reasoning_effort`
   - Use the Codex effort mapping when a numeric effort is provided: `1` = `low`, `2` = `medium`, `3` = `high`, `4` = `xhigh`.
   - Use `medium` for normal agent workflows.
   - Use `high` only for security, architecture, complex debugging, or PR review.
   - Use `xhigh` only when the user explicitly requests extra-high reasoning or the task is unusually high risk.
   - Omit only when `model` is also omitted.

5. `sandbox_mode`
   - Use `read-only` for explorer/reviewer/research agents.
   - Use `workspace-write` for implementation/fixer/generator agents.
   - Omit if the parent session should decide.

## Output requirements

Always output:

1. The target path.
2. The full TOML content in a code block.
3. A shell command to create the file.
4. A short usage example showing how to ask Codex to spawn/use the agent.

If the user asks to create files in the repository, write the `.toml` file directly.

## TOML template

```toml
name = "agent_name"
description = "Trigger words first. Explain exactly when this agent should be used."
model_reasoning_effort = "medium"
sandbox_mode = "read-only"

developer_instructions = """
You are a specialized Codex custom agent.

Mission:
- Do one narrow job extremely well.
- Stay within the requested scope.
- Prefer concrete evidence from the repository over assumptions.

Workflow:
1. Understand the user's task and constraints.
2. Inspect only the files or commands needed for the task.
3. Execute the smallest safe set of changes or analysis.
4. Validate the result when possible.
5. Return a concise final report with changed files, decisions, and next steps.

Rules:
- Do not drift into adjacent tasks.
- Do not perform destructive actions unless explicitly requested.
- Do not expose secrets, tokens, or private credentials.
- Do not include AI attribution in generated commits, PRs, or code comments.
- If blocked, explain the blocker and provide the safest next command.

Output:
- Keep the response concise.
- Include exact files changed or inspected.
- Include validation commands and results when available.
"""
```

## Conversion rules from legacy agent markdown

When converting legacy provider-style agent markdown:

- Drop YAML fields that Codex does not use directly, such as provider-specific `tools` lists.
- Convert legacy fast-lane model names to Codex only when there is an explicit mapping. In this workspace, map legacy fast-lane models to `model = "gpt-5.3-codex-spark"` and `model_reasoning_effort = "low"`.
- Preserve the role, strict workflow, safety rules, command sequences, output format, and error handling inside `developer_instructions`.
- Convert “Use when...” language into the Codex `description`.
- Keep command blocks inside `developer_instructions` if they are part of the workflow.
- Keep the agent narrow; do not merge multiple unrelated roles into one agent.

## Naming conventions

- File path: `.codex/agents/<kebab-name>.toml`
- TOML `name`: `<snake_name>`
- Description: short, trigger-focused, under 200 characters when possible.
- Developer instructions: explicit, imperative, and task-focused.

## Validation checklist

Before finalizing, verify:

- TOML has valid quoted strings.
- Multi-line instructions use triple quotes.
- `name`, `description`, and `developer_instructions` exist.
- Any agent with `model` also has `model_reasoning_effort`.
- Agent has one clear responsibility.
- Any write-capable agent has safety rules.
- Any Git/PR agent forbids AI attribution.
- Any reviewer/explorer agent uses read-only sandbox when specified.

## File creation command pattern

Use this pattern for project-scoped agents:

```bash
mkdir -p .codex/agents
cat > .codex/agents/<agent-name>.toml <<'TOML'
name = "agent_name"
description = "..."

developer_instructions = """
...
"""
TOML
```

Use this pattern for global agents:

```bash
mkdir -p ~/.codex/agents
cat > ~/.codex/agents/<agent-name>.toml <<'TOML'
name = "agent_name"
description = "..."

developer_instructions = """
...
"""
TOML
```

## Usage examples

Explicit prompt example:

```text
Use the git_manager agent to stage, commit, and push the current changes.
```

Subagent workflow example:

```text
Review this branch against main. Spawn reviewer, docs_researcher, and security_reviewer agents, wait for all results, then summarize the findings.
```
