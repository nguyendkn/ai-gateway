---
name: fix
description: Diagnose and fix bugs, failing tests, type errors, CI failures, UI defects, logs, regressions, and runtime issues.
---

# Fix

Use this skill when the user reports something broken and wants it fixed.

## Workflow

1. Reproduce or inspect the failure evidence.
2. Identify the root cause before editing.
3. Make the smallest safe fix.
4. Add or update regression coverage when appropriate.
5. Run the narrowest meaningful validation, then broader checks if risk warrants it.

## Routing

- CI logs: use `plan-ci` first when the user asks for a plan, otherwise inspect and fix directly.
- Deep debugging: use `gitnexus-debugging` or `debugger`.
- Parallel independent fixes: use `parallel-implement`.
- UI/browser bugs: use `qa`, `qa-only`, or `design-review` when available.

## Rules

- Do not patch symptoms before root cause is understood.
- Do not hide failures behind broad fallbacks.
- Do not ignore failing checks.
