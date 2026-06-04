---
name: git-commit
description: Stage all repository files and create a Git commit with a required subject and body through the git-manager agent using model haiku. Use when the user asks Codex to commit current changes, make a Git commit, or stage and commit work without pushing.
---

# Git Commit

## Workflow

Use the `git-manager` agent to stage all files and create the commit. When calling the Task tool, always specify:

```text
model: haiku
```

The task prompt must instruct the agent to:

1. Stage all tracked, modified, deleted, and untracked files.
2. Inspect the staged diff before committing.
3. Create a commit with both a concise subject and a description body.
4. Avoid pushing to any remote repository.
5. Avoid AI attribution, generated-by trailers, or co-author trailers.

## Commit Message Requirements

The commit message must never be subject-only. Include a body that briefly explains what changed and why. Use an imperative subject, for example:

```text
Add repository contributor guide

Document the scaffolded project structure, current development commands,
testing expectations, and security guidance for future contributors.
```

## Completion Report

After the agent finishes, report the commit hash, commit subject, and whether anything was left unstaged. Also state that no push was performed.

If the Task tool or `git-manager` agent is unavailable, stop and report that blocker instead of committing directly.
