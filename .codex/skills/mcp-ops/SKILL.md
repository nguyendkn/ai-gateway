---
name: mcp-ops
description: Discover, inspect, plan, or safely use MCP servers and tools available in the current Codex session.
---

# MCP Ops

Use this skill for MCP-related work.

## Workflow

1. Identify the requested MCP capability.
2. Inspect available app/MCP tools in the current session.
3. Select the safest Codex-native tool path.
4. Execute read-only operations directly when safe.
5. Ask only for credentialed, production, or destructive operations.

## Rules

- Use `mcp-manager` for delegated analysis.
- Do not call external provider CLIs.
- Do not expose secrets.
- Report unavailable servers or tools clearly.
