---
name: git-commit-push
description: Stage all changes, create a meaningful conventional commit with both subject and description body, and push the current branch. Use when the user asks to commit and push all code, stage/commit/push the current branch, or run a fast git-manager commit-and-push workflow.
---

# Git Commit Push

## Operating Rules

- Use `git_manager` when available.
- Use the fastest available execution path. If a model override is supported, request `haiku` for the `git_manager` task.
- Stage all repository changes.
- Create a meaningful conventional commit based on the staged changes.
- Push the current branch to the configured remote.
- Never include AI attribution in commit subject, body, trailers, or push output.
- Stop if secret patterns are detected.
- Do not force push unless the user explicitly asks.

## Commit Message Requirements

Every commit must include:

- A conventional subject line under 72 characters.
- A description body explaining what changed and why.

Example:

```text
feat(auth): add token refresh flow

Add refresh-token handling for authenticated sessions.
Update validation paths so expired access tokens can recover cleanly.
```

Do not create subject-only commits.

## Workflow

1. Delegate to `git_manager` with an explicit commit-and-push request:

```text
Stage all files, check for secrets, create a conventional commit with both subject and body, and push the current branch. Use haiku model if model selection is available.
```

2. If delegation is unavailable, run the equivalent workflow locally:
   - `git add -A`.
   - Inspect staged stats with `git diff --cached --stat`.
   - Check for secret patterns in `git diff --cached`.
   - Generate a conventional subject and body from the staged diff.
   - Commit with `git commit -m "subject" -m "body"`.
   - Push with `git push`, or `git push -u origin HEAD` when upstream is missing.

3. If there are no changes, report `No changes to commit`.
4. If push fails, report the failure and the safest next command.

## Output

Keep output concise:

```text
staged: N files (+A/-D lines)
security: passed
commit: <hash> <subject>
pushed: yes
```

If blocked, report the blocker and do not commit.
