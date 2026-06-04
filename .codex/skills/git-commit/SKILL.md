---
name: git-commit
description: Stage all repository files and create a local Lore-compliant Git commit without pushing. Use when the user asks Codex to commit current changes, make a Git commit, or stage and commit work without pushing.
---

# Git Commit

## Workflow

Use the fastest available Codex-native path:

1. If a native Git agent is available in the current surface, use it with Codex-native routing. Prefer `git_manager` when exposed, otherwise `git-master`.
2. If no native Git agent is callable from the current surface, execute the same workflow directly with local git commands.
3. Do not require provider-specific task tools or non-Codex model names.

The workflow must:

1. Stage all tracked, modified, deleted, and untracked files.
2. Inspect the staged diff before committing.
3. Check the staged diff for likely secrets before committing.
4. Create a commit with both a concise subject and a description body.
5. Avoid pushing to any remote repository.
6. Avoid AI attribution, generated-by trailers, or co-author trailers.

Suggested direct command sequence:

```bash
git add -A
git diff --cached --stat
git diff --cached --check
git diff --cached | rg -i "(api[_-]?key|token|password|secret|private[_-]?key|credential)" -C2
```

If the secret scan returns real secret material, stop and report the blocker. If it returns only benign documentation or configuration wording, explain why it is benign and continue.

If there are no staged changes after `git add -A`, report `No changes to commit` and stop.

## Commit Message Requirements

The commit message must never be subject-only. Follow the repository Lore Commit Protocol from `AGENTS.md`:

```text
<intent line: why the change was made, not what changed>

<optional concise body: constraints and approach rationale>

Constraint: <external constraint that shaped the decision>
Rejected: <alternative considered> | <reason for rejection>
Confidence: <low|medium|high>
Scope-risk: <narrow|moderate|broad>
Directive: <forward-looking warning for future modifiers>
Tested: <what was verified>
Not-tested: <known gaps in verification>
```

Use trailers only when they add decision context, but always include at least:

1. `Confidence:`
2. `Scope-risk:`
3. `Tested:`
4. `Not-tested:`

Example:

```text
Keep Codex orchestration from routing through legacy providers

Normalize local skills around Codex-native CLI commands and remove stale
advisor/backend references that caused workflow blockers.

Confidence: high
Scope-risk: narrow
Tested: json/toml parse, grep backend scan, codex exec smoke test
Not-tested: full OMX tmux runtime
```

Commit with a subject and body, for example:

```bash
git commit -m "Keep Codex orchestration from routing through legacy providers" \
  -m "Normalize local skills around Codex-native CLI commands and remove stale advisor/backend references that caused workflow blockers." \
  -m "Confidence: high" \
  -m "Scope-risk: narrow" \
  -m "Tested: json/toml parse, grep backend scan, codex exec smoke test" \
  -m "Not-tested: full OMX tmux runtime"
```

## Completion Report

After the commit finishes, report:

1. Commit hash.
2. Commit subject.
3. Whether anything was left unstaged.
4. Avoid pushing to any remote repository.
5. Validation performed before the commit.

Always state that no push was performed.
