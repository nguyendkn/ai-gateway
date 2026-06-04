---
name: code-fast
description: Implement a small or medium scoped code change quickly with local inspection, focused edits, and targeted validation.
---

# Code Fast

Use this skill for direct implementation when requirements are clear and the change is not broad or high-risk.

## Workflow

1. Check `git status --short`.
2. Inspect only the relevant files, tests, and manifests.
3. State a short implementation plan.
4. Edit with `apply_patch`.
5. Run targeted validation first; run broader checks when risk warrants it.
6. Report changed files, validation, and remaining risks.

## Rules

- Prefer existing patterns and helpers.
- Do not skip validation unless there is a concrete blocker.
- Do not weaken tests or fake behavior.
- If the user explicitly says no tests, still run compile/type/static checks when available and state the gap.
