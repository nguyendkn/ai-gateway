---
name: ask
description: "[OMX] Ask a local Codex advisor CLI and capture a reusable artifact"
---

# Ask (Local Advisor CLI)

Use a local Codex advisor CLI for focused questions, reviews, brainstorming, or second opinions.

## Usage

```bash
$ask codex <question or task>
codex exec -m gpt-5.3-codex-spark -c 'model_reasoning_effort="low"' --ephemeral --sandbox read-only "<question or task>"
```

## Backend selection

- Use `codex` for advisor requests in this Codex workspace.
- Default to effort `1` for fast advisor requests unless the user asks for deeper reasoning.
- If the Codex CLI is unavailable, explain that a local CLI is required.

## Effort mapping

Use Codex `model_reasoning_effort` values:

| Level | Meaning | Codex value |
| --- | --- | --- |
| 1 | Low | `low` |
| 2 | Medium | `medium` |
| 3 | High | `high` |
| 4 | Extra high | `xhigh` |

## Local CLI commands

Codex:

```bash
codex exec -m gpt-5.3-codex-spark -c 'model_reasoning_effort="low"' --ephemeral --sandbox read-only "{{ARGUMENTS}}"
```

If needed, adapt to the user's installed Codex CLI variant while keeping local execution as the default path. Do not silently switch to an MCP or remote provider when the local binary is missing.

## Artifact requirement

After local execution, save a markdown artifact to:

```text
.omx/artifacts/ask-<backend>-<slug>-<timestamp>.md
```

Minimum artifact sections:
1. Original user task
2. Backend and final prompt sent to the CLI
3. Raw CLI output
4. Concise summary
5. Action items / next steps

Task: {{ARGUMENTS}}
