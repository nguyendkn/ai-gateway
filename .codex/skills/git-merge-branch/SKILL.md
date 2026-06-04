---
name: git-merge-branch
description: Merge code from one Git branch to another by syncing the target branch, merging the remote tracking source branch with --no-ff, resolving conflicts, and pushing the result. Use when Codex is asked to merge, sync, or promote committed and pushed branch changes into main or another target branch.
---

# Git Merge Branch

## Overview

Merge a source branch into a target branch using `origin/{FROM_BRANCH}` as the merge source. This preserves the invariant that only committed and pushed source changes are merged, never local uncommitted work.

## Arguments

- `TO_BRANCH`: first argument; default to `main`.
- `FROM_BRANCH`: second argument; default to the current branch captured before checking out `TO_BRANCH`.

If `FROM_BRANCH` is omitted and the repository is in detached HEAD state, ask the user for the source branch.

## Workflow

1. Capture and inspect state:

```bash
git branch --show-current
git status --short
```

If local changes would block checkout, pull, merge, or push, stop and ask. Do not stash, commit, discard, or include local work unless the user explicitly asks.

2. Sync with remote:

```bash
git fetch origin
git checkout {TO_BRANCH}
git pull origin {TO_BRANCH}
```

After fetching, verify the source tracking branch exists:

```bash
git rev-parse --verify --quiet origin/{FROM_BRANCH}
```

If `origin/{FROM_BRANCH}` does not exist, stop and report that the source branch must be pushed first.

3. Merge from the remote tracking branch:

```bash
git merge origin/{FROM_BRANCH} --no-ff -m "merge: {FROM_BRANCH} into {TO_BRANCH}"
```

Always merge `origin/{FROM_BRANCH}`, not `{FROM_BRANCH}`. This is the core safety rule for excluding local unpushed source changes.

4. Resolve conflicts if needed:

```bash
git status --short
```

Resolve conflicted files manually. After all conflicts are resolved:

```bash
git add .
git commit
```

If no conflicts occur, the merge command creates the merge commit directly.

5. Push the merged target branch:

```bash
git push origin {TO_BRANCH}
```

## Notes

- Always fetch and pull the latest remote target state before merging.
- Do not replace the remote-source merge with a local branch merge unless the user explicitly requests that risk.
- If the surrounding task requires `gh` and it is not available, instruct the user to install and authorize GitHub CLI first.
