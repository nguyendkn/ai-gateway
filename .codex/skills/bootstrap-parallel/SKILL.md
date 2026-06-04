---
name: bootstrap-parallel
description: Bootstrap a new project with parallel research, stack selection, design planning, approval-gated wireframes, parallel implementation phases, real testing, review, documentation, onboarding, and optional conventional commits. Use when the user asks for parallel project bootstrap, ultrathink parallel bootstrap, or fast MVP creation with subagents and strict phase execution.
---

# Bootstrap Parallel

## Operating Rules

- Use user requirements as source of truth.
- Be blunt about feasibility, complexity, risks, and trade-offs.
- Apply YAGNI, KISS, and DRY.
- Activate relevant skills and agents when available.
- Follow `AGENTS.md`, `CLAUDE.md`, `docs/code-standards.md`, and development rules when present.
- Keep research reports at or under 150 lines.
- Sacrifice grammar for concision in reports.
- Do not fake data or weaken checks to pass tests.
- Do not expose or commit secrets.
- Do not push. Ask before commit; use `git_manager` only after user says yes.

This skill is allowed to implement code because its purpose is project bootstrap.

## Inputs

Capture `[user-requirements]` exactly. If requirements are too vague to choose a stack or build safely, ask one focused question. Otherwise proceed with explicit assumptions.

## Workflow

### 1. Git Init

1. Check whether Git is initialized.
2. If not, use `git_manager` when available; otherwise run `git init -b main`.

### 2. Research

Use parallel agents only when multi-agent tools are available. If unavailable, perform the same work locally with source limits.

- Launch up to 2 `researcher` agents in parallel for requirements validation, challenges, risks, and solution options.
- Each agent reads max 5 sources.
- Require concise reports with citations.

### 3. Tech Stack

- Use `planner` plus parallel `researcher` agents when useful to compare stack options.
- Write chosen stack and rationale under `docs/`, usually `docs/tech-stack.md`, at or under 150 lines.
- Choose simplest stack that satisfies requirements, deployment constraints, security, and maintainability.

### 4. Wireframe And Design

Run `ui_ux_designer` and researcher-style work in parallel where possible:

- Research style, trends, fonts, colors, spacing, layout positions, and asset needs.
- Describe assets clearly enough for later image generation.
- Create or update `docs/design-guidelines.md`.
- Generate HTML wireframes under `docs/wireframe/`.
- Generate logo with image tools if no logo exists and brand identity matters.
- Capture screenshots with browser tooling when available; save under `docs/wireframes/`.
- Ask user to approve the design before implementation. If rejected, iterate until approved.

### 5. Parallel Planning And Implementation

1. Trigger or emulate `/vk:plan:parallel <detailed-instruction>` to create a parallel-executable plan.
2. Read `plan.md` for dependency graph, phases, ownership, and execution strategy.
3. Launch multiple `fullstack_developer` agents in parallel only for phases that:
   - Have no dependency conflicts.
   - Have disjoint file ownership.
   - Can run safely without shared-write overlap.
4. Pass each agent the phase file path and environment details.
5. Use `ui_ux_designer` for frontend/UI phases and asset generation/editing.
6. Main agent integrates results, resolves non-overlapping outputs, and runs typecheck/build.
7. If no parallel tooling is available, execute phases locally in dependency order.

### 6. Testing

- Write real tests for behavior and edge cases. No fake data solely to pass tests.
- Use `tester` when available.
- If tests fail, use `debugger` when available, fix, then repeat.
- Continue until tests pass or blockers are explicit.

### 7. Code Review

- Use `code_reviewer` after implementation.
- Fix critical findings, then retest.
- Repeat until no critical blockers remain.

### 8. Documentation

Use `docs_manager` or docs skills to create or update:

- `docs/README.md` under 300 lines.
- `docs/project-overview-pdr.md`.
- `docs/code-standards.md`.
- `docs/system-architecture.md`.
- `docs/project-roadmap.md` via `project_manager` when available.

Keep `docs/` as source of truth.

### 9. Onboarding

Guide the user one question at a time for API keys, env vars, provider choices, and local configuration. Wait for each answer before asking the next.

### 10. Final Report

Report:

- Summary of what was built.
- Key decisions and trade-offs.
- Files changed.
- Commands run and results.
- Tests, review, and docs status.
- Setup guide and next steps.
- Unresolved questions.

Ask whether to commit. If yes, use `git_manager` to commit without pushing.
