---
name: primary-workflow
description: Default implementation workflow for planned code changes. Use when Codex is asked to implement features, fix approved bugs, modify existing code, add tests, run validation, delegate planner/tester/code-reviewer/docs-manager/debugger agents, and finish only after tests pass or blockers are explicit.
---

# Primary Workflow

Use this skill for normal implementation work that needs planning, code changes, tests, review, and docs. Keep execution token-efficient and high quality.

## Rules

- Analyze the skills catalog and activate only skills/agents that materially help.
- Follow `AGENTS.md`, `.codex/config.toml`, repo docs, architecture, and coding standards.
- Respect user-owned changes. Do not revert unrelated work.
- Update existing files directly. Do not create duplicate "enhanced" files.
- Keep code clean, readable, maintainable, and aligned with existing architecture.
- Handle edge cases and error paths.
- Do not use fake data, mocks, cheats, temporary bypasses, or weakened assertions just to pass build, tests, or CI.
- Do not ignore failing tests.
- Do not expose or commit secrets.

## Workflow

### 1. Planning

- Before coding, use `planner` when available to create an implementation plan with TODO tasks under `plans/`.
- During planning, use up to 2 `researcher` agents in parallel for distinct relevant topics when the task is non-trivial, architecture-heavy, security-sensitive, or depends on current external behavior.
- If subagents are unavailable, perform equivalent local planning and research.
- Do not start implementation until the plan is clear enough to execute.
- For bug reports, regressions, server issues, or CI/CD failures, follow repo investigation-first rules: trace root cause and present plan before implementing unless user already approved the plan.

### 2. Code Implementation

- Follow the planner plan.
- Make the smallest coherent changes.
- Prefer existing patterns, APIs, helpers, configs, and tests.
- Use `apply_patch` for manual edits.
- After creating or modifying code files, run compile/typecheck/build commands discovered from repo scripts/docs.
- Maintain API contracts and backward compatibility unless the task explicitly requires a breaking change.
- Document breaking changes.

### 3. Testing

- Write or update real tests for new behavior, error scenarios, edge cases, and relevant regressions.
- Delegate to `tester` when available to run tests and analyze the report.
- Fix failing tests according to evidence and tester recommendations.
- Re-run `tester` after fixes.
- Finish only when relevant tests pass or a concrete external blocker is reported.

### 4. Code Quality

- After implementation and tests, delegate to `code-reviewer` when available.
- Fix critical/high review findings.
- Add comments only for complex logic where they help future maintainers.
- Optimize for performance and maintainability without speculative refactors.

### 5. Integration And Docs

- Ensure changes integrate with existing code, services, configs, and API contracts.
- Delegate to `docs-manager` when docs in `docs/` need updates because behavior, API contracts, config, architecture, setup, or operations changed.
- Update history logs or lesson-learn artifacts when repo instructions require them.

### 6. Debugging Loop

- For server bugs, CI/CD issues, test failures, or unclear runtime behavior, delegate to `debugger` when available.
- Read debugger report, implement the fix, then return to testing.
- If `tester` reports failures, repeat testing/debugging until passing or blocked.

### 7. Final Report

- Summarize:
  - Plan followed.
  - Files changed.
  - Tests/build/compile commands and results.
  - Review status.
  - Docs updates.
  - Remaining blockers or unresolved questions.
- Do not commit or push unless user explicitly asks.

## Success Criteria

- Planner output exists or local plan is stated.
- Requested behavior is implemented in existing files.
- Relevant compile/typecheck/build/test checks pass.
- Code review has no blocking unresolved findings when run.
- Docs are updated when required.
- Final report is concise and evidence-based.
