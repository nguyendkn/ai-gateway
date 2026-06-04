---
name: sql
description: Generate, validate, manage, optimize, or explain SQL and text-to-SQL semantic-layer workflows.
---

# SQL

Use this skill for SQL generation, validation, optimization, schema inspection, or semantic-layer text-to-SQL work.

## Workflow

1. Determine whether the task is query generation, optimization, schema setup, glossary management, or validation.
2. Inspect schema sources or semantic-layer files if available.
3. Resolve business terms before writing SQL.
4. Generate SQL with explanation.
5. Validate against schema or local validators when available.

## Rules

- Use `text-to-sql` for semantic-layer work.
- Use `database-admin` for performance, indexes, migrations, backups, or permissions.
- Never execute SQL against a live database without explicit approval.
- Do not expose credentials or connection strings.
