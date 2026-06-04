---
name: parallel-implement
description: Plan tasks into dependency-aware parallel phases and execute them with fullstack-developer agents. Use when the user asks to ultrathink parallel, create a parallel implementation plan, run parallel phases, or implement a task list with fullstack-developer subagents, tester, debugger, code-reviewer, docs-manager, project-manager, and optional git-manager commit.
---

# Parallel Implement

## Operating Rules

- Treat `[tasks]` as source of truth.
- Activate relevant skills and agents when available.
- Keep reports concise; sacrifice grammar for concision.
- Apply YAGNI, KISS, and DRY.
- Respect user-owned changes. Never revert unrelated work.
- Do not fake data, mock solely to pass tests, weaken checks, or ignore failures.
- Do not expose or commit secrets.
- Ask before commit. Do not push unless explicitly requested.

This skill is allowed to implement code because its purpose is task execution.

## Workflow

### 1. Research Optional

Use research only when tasks are complex, ambiguous, architecture-heavy, security-sensitive, or use unfamiliar dependencies.

- Use max 2 `researcher` agents in parallel when multi-agent tools are available.
- Use `/vk:scout:ext` if available; otherwise use `rg`, `rg --files`, manifests, docs, and targeted reads.
- Keep research reports at or under 150 lines.
- If research is unnecessary, skip it and state why in final report.

### 2. Parallel Planning

1. Trigger or emulate `/vk:plan:parallel <detailed-instruction>`.
2. Produce or locate a plan directory with:
   - `plan.md` overview.
   - Dependency graph.
   - Execution strategy.
   - File ownership matrix.
   - Phase files: `phase-XX-*.md`.
3. Read `plan.md` before implementation.
4. Refuse parallel execution if file ownership is missing, overlapping, or ambiguous.

### 3. Parallel Implementation

Use `fullstack_developer` agents only for phases that can run concurrently.

- Example: `Phases 1-3 parallel` means launch 3 `fullstack_developer` agents at once.
- Pass each agent:
  - Phase file path, for example `{plan-dir}/phase-01-api.md`.
  - Environment info and validation commands.
  - Strict file ownership boundaries.
- Wait for all phases in a parallel batch before starting dependent phases.
- Run sequential phases one at a time.
- If subagent tools are unavailable, implement phases locally in dependency order.
- If a phase reports ownership conflict, stop that phase and resolve before continuing.

### 4. Testing

- Write or update real tests for implemented behavior.
- Use `tester` for full relevant test suite when available.
- If tests fail, use `debugger` when available, fix root cause, then repeat.
- Continue until tests pass or blockers are explicit.

### 5. Code Review

- Use `code_reviewer` for all non-trivial changes.
- If critical issues exist, fix and retest.
- Document accepted residual risks.

### 6. Project Management And Docs

If the user approves docs/tracking updates or the task requires them:

- Use `project_manager` and `docs_manager` in parallel when available.
- Update plan files, docs, roadmap, and relevant status.
- If updates are rejected or unclear, skip and report.

### 7. Final Report

Report:

- Parallel batches executed.
- Phase status and files changed.
- Tests run and results.
- Review status.
- Docs/tracking updates.
- Guide to get started or verify.
- Unresolved questions.

Ask whether to commit. If yes, use `git_manager` for conventional commit without pushing.

## Success Criteria

- Plan has dependency graph and file ownership matrix.
- Parallel phases use disjoint ownership.
- Dependent phases wait for prerequisites.
- Tests are real and passing, or blockers are explicit.
- Code review has no critical unresolved findings.
- Final report is concise and actionable.
