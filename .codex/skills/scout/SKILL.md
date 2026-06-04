---
name: scout
description: Quickly locate relevant files, symbols, tests, configs, and implementation surfaces in the current repository.
---

# Scout

Use this skill for fast repo-local discovery before planning or editing.

## Workflow

1. Restate what needs to be found.
2. Use `rg`, `rg --files`, and focused file reads.
3. Return a grouped file map with relevance notes.
4. Identify missing coverage or likely next searches.

## Rules

- Prefer the `scout` or `explore` agent for delegated read-only discovery.
- Do not edit files.
- Do not use external provider CLIs.
- Keep output short and actionable.
