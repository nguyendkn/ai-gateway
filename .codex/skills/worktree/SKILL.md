---
name: worktree
description: Create, inspect, clean up, or plan Git worktrees for isolated branches and parallel development.
---

# Worktree

Use this skill for Git worktree operations.

## Workflow

1. Run `git status --short` and `git worktree list`.
2. Identify target branch, slug, base branch, and files/env constraints.
3. For creation, use `git worktree add` with a clear path and branch.
4. For cleanup, verify the worktree is safe to remove before running removal commands.
5. Report paths, branch names, and remaining manual steps.

## Rules

- Do not overwrite or delete user work.
- Do not remove a dirty worktree without explicit approval.
- Do not copy secrets into worktrees.
- Use team/worktree OMX runtime only when explicitly active and needed.
