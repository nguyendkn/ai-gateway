---
name: cook-auto
description: Plan and implement feature tasks automatically from a user-provided task list. Use when Codex is asked to "cook", implement a feature end-to-end, turn tasks into a plan, make code changes, validate with tests/builds, review, and then ask whether to commit.
---

# Cook Auto

Use this skill for feature work where the user wants Codex to plan, implement, validate, and report with minimal back-and-forth. Keep the work surgical and evidence-based.

## Operating Rules

- Read top-level guidance first: `AGENTS.md`, `.codex/config.toml`, and relevant `docs/` files when present.
- Activate relevant local skills and agents when useful: `planner`, `fullstack-developer`, `tester`, `debugger`, `code-reviewer`, `docs-manager`, `project-manager`, `git-manager`, GitNexus skills, and domain-specific skills.
- If expected slash commands such as `/plan`, `/code`, or `/git:cm` are unavailable, execute their intent directly with Codex tools and local agents.
- Respect user-owned changes. Never revert unrelated work.
- Keep changes scoped to the requested tasks and repo standards.
- Do not fake data, weaken tests, skip failing checks, or hide uncertainty.
- Do not expose or commit secrets.
- Keep reports concise. Sacrifice grammar for concision. List unresolved questions at the end.

## Workflow

### 1. Intake

- Treat `[tasks]` as the source of truth.
- If tasks are ambiguous but a safe assumption exists, proceed and document the assumption.
- Ask one blocking question only when implementation would be risky or impossible without it.
- Check `git status --short` before editing.

### 2. Plan

- Build a short implementation plan from the tasks before changing code.
- For non-trivial work, include:
  - Goal and acceptance criteria.
  - Files or modules likely touched.
  - Dependencies, risks, and validation commands.
  - Test strategy.
- Use `planner` or `brainstormer` when architecture, product behavior, or implementation strategy is not obvious.
- For this repository, follow required issue-work and harness architecture planning rules when the task is a bug, regression, failed session, hanging run, or investigation.

### 3. Implement

- Implement in the smallest coherent increments.
- Prefer existing patterns, helpers, APIs, configs, and test style.
- Run required impact analysis before editing symbols when local GitNexus instructions require it.
- Use `apply_patch` for manual edits.
- Update docs only when behavior, config, API, architecture, or user workflow changes.

### 4. Validate

- Discover validation commands from manifests, Makefiles, README, docs, or existing CI.
- Run targeted checks first, then broader checks when risk warrants it.
- Use `tester` when available for test/build/coverage validation.
- If failures appear, use `debugger` when available to identify root cause, then fix and rerun.
- Repeat until checks pass or blockers are explicit.

### 5. Review

- Use `code-reviewer` when changes are non-trivial, high-risk, shared, security-sensitive, or user-facing.
- Fix critical and high findings.
- Rerun relevant validation after fixes.

### 6. Docs And Tracking

- Use `docs-manager` for documentation updates when needed.
- Use `project-manager` when a plan, roadmap, or multi-agent status needs updating.
- Create history-log or lesson-learn entries when repo instructions require them.

### 7. Final Handoff

- Report:
  - What changed.
  - Files changed.
  - Validation commands and results.
  - Review status.
  - Remaining risks or unresolved questions.
- Ask whether the user wants to commit. If yes, use `git-manager` or the `git-commit` skill when available.
- Do not push unless explicitly requested.

## Success Criteria

- Requested tasks are implemented or blockers are clearly explained.
- Tests/build/lint/typecheck relevant to the change pass, or failures are reported with root cause/status.
- Code review has no blocking findings when review is run.
- Docs/tracking are updated when required.
- Final response gives the user a clear next step.
