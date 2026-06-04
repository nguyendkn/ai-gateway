---
name: cook-auto-fast
description: Fast local feature implementation with no external research. Use when Codex is asked to scout the current codebase, create a quick plan, implement requested tasks, run relevant validation, and skip formal code review unless risk requires it.
---

# Cook Auto Fast

Use this skill for small to medium feature tasks where the user wants fast execution: scout local code, plan briefly, implement, validate, report. Do not browse or perform external research unless the user explicitly overrides "no research".

## Rules

- Treat `[tasks-or-prompt]` as the source of truth.
- Follow `AGENTS.md`, `.codex/config.toml`, local docs, and development rules when present.
- Analyze local skills in `.codex/skills/` when present; activate only skills that materially help.
- Apply YAGNI, KISS, and DRY. Prefer minimal changes over speculative architecture.
- Preserve user changes. Do not revert unrelated work.
- Do not use fake data, weaken checks, ignore failures, or hide uncertainty.
- Do not expose or commit secrets.
- Keep reports concise. Sacrifice grammar for concision. List unresolved questions at the end.

## Workflow

### 1. Scout

- Check `git status --short`.
- Use local scout behavior: inspect relevant docs, plans, configs, manifests, tests, and code.
- Prefer GitNexus query/context/impact tools when available and required by repo instructions.
- Prefer `rg` and `rg --files` for file discovery.
- Do not perform internet research.
- Summarize only the code/resources needed to implement the task.

### 2. Fast Plan

- Create a short implementation plan before edits.
- Include:
  - Goal and acceptance criteria.
  - Files/symbols likely touched.
  - Risks and assumptions.
  - Validation commands.
- For bug/regression/investigation work, obey repo instructions that require root-cause tracing and human approval before implementation.
- If a slash command such as `/plan:fast` is unavailable, execute its intent directly.

### 3. Implement

- Implement the plan directly in the main agent unless a local specialist agent is clearly useful.
- Use `apply_patch` for manual edits.
- Run required impact analysis before editing symbols when local instructions require it.
- Match existing code style and architecture.
- Do not update docs unless the change affects behavior, configuration, APIs, architecture, or user workflow.
- If a slash command such as `/code "skip code review step" <plan>` is unavailable, execute its intent directly.

### 4. Validate

- Discover commands from Makefiles, manifests, README, docs, and CI config.
- Run the narrowest meaningful checks first: typecheck, lint, unit tests, targeted tests, build.
- Use `tester` or `debugger` agents only when available and worth the overhead.
- Fix failures and rerun relevant checks until passing or blocked.

### 5. Handoff

- Report:
  - What changed.
  - Files changed.
  - Commands run and results.
  - Skipped checks with reason.
  - Remaining risks and unresolved questions.
- Ask whether the user wants a git commit. If yes, use `git-manager` or `git-commit` when available.
- Do not push unless explicitly requested.

## Success Criteria

- Requested tasks implemented or blocked with concrete reason.
- Relevant validation passes or failures are explained.
- No external research performed.
- No unrelated files changed.
- User has a clear next step.
