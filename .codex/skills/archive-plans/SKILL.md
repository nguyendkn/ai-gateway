---
name: archive-plans
description: Read one or more plan directories, optionally write concise journal entries, then archive or permanently delete selected plans after explicit confirmation. Use when the user asks to journal plans, archive plans, clean up plans, archive all completed plans, or move/delete plan folders under plans/.
---

# Archive Plans

## Operating Rules

- Treat `plans/` as the plan source and `docs/journals/` as the journal destination.
- If the user provides a path, operate only on that path unless they later expand scope.
- If no path is provided, inspect all direct plan directories under `plans/`, excluding `plans/archive/`.
- Ask only genuine decision questions.
- Get explicit confirmation before moving or permanently deleting plan folders.
- Prefer moving to `plans/archive/` over deletion.
- Do not permanently delete anything without a clear user choice.
- Sacrifice grammar for concision in outputs.
- List unresolved questions at the end of the final report.

## Workflow

### 1. Resolve Plans

1. If arguments include a path, use that plan file or directory.
2. Otherwise list direct children of `plans/`.
3. Exclude `plans/archive/` from default active-plan discovery.
4. For each plan directory, read:
   - `plan.md` fully enough to extract title, status, created date, phases, and summary.
   - First 20 lines of each `phase-*.md` to understand progress and status.
5. Count LOC for plan files and later journal files with `wc -l`.

### 2. Optional Journals

Ask whether to create journal entries. Skip if user says no.

If yes:

- Use `journal_writer` subagent when available; otherwise write locally.
- Create concise Markdown entries in `docs/journals/`.
- Focus on important events, key changes, decisions, impact, and outcome.
- Avoid duplicating full plan content.
- Use stable filenames such as `YYYYMMDD-plan-slug-journal.md`.

If a `/vk:journal` command exists, it may be used; otherwise emulate it directly.

### 3. Archive Decision

Ask the user to choose what to archive:

- Specific selected plans.
- All completed plans only.
- All resolved plans.
- Cancel.

Then ask how to handle selected plans:

- Move to `plans/archive/`.
- Permanently delete.
- Cancel.

Permanent delete requires explicit confirmation after the selection is known.

### 4. Archive Or Delete

For move:

- Create `plans/archive/` if missing.
- Move selected plan directories into `plans/archive/`.
- If a target already exists, avoid overwrite by appending a short suffix or ask if ambiguity is risky.

For delete:

- Use `rm -rf` only after explicit confirmation.
- Delete only the selected plan directories.
- Never delete `plans/` or `plans/archive/` itself.

### 5. Commit Decision

Ask whether to commit changes:

- Stage and commit only.
- Commit and push.
- Skip commit.

Use project Git skills or agents when available:

- Commit only: `git_commit` or `git_manager`.
- Commit and push: `git_commit_push` or `git_manager`.

If slash commands such as `/vk:git:cm` or `/vk:git:cp` exist, they may be used; otherwise emulate their intent directly.

## Final Report

Report:

- Number of plans archived.
- Number of plans permanently deleted.
- Table of archived/deleted plans: title, status, created date, LOC, action.
- Table of journal entries created: title, status/date when available, LOC, path.
- Commit status.
- Unresolved questions.
