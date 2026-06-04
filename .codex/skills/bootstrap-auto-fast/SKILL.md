---
name: bootstrap-auto-fast
description: Quickly plan, scaffold, implement, test, review, document, commit, and onboard a new project from user requirements. Use when the user asks to bootstrap a new project automatically, create an MVP from requirements, run a fast end-to-end project setup, or orchestrate research, design, implementation, testing, review, docs, and git commit without pushing.
---

# Bootstrap Auto Fast

## Operating Rules

- Use the user's requirements as the source of truth.
- Be blunt about feasibility, overengineering, risks, and trade-offs.
- Apply YAGNI, KISS, and DRY throughout.
- Activate relevant project skills and agents when available.
- Follow `CLAUDE.md`, `AGENTS.md`, `docs/code-standards.md`, and development rules when present.
- Do not expose secrets, tokens, credentials, or private data.
- Do not push to remote unless the user explicitly asks.
- Keep reports concise; sacrifice grammar for concision if needed.
- List unresolved questions at the end of reports.

This workflow is allowed to implement code because the skill's purpose is project bootstrap. When subagent tools are unavailable, do the same workflow locally in smaller sequential steps.

## Inputs

Capture `[user-requirements]` exactly. If requirements are too vague to choose a stack or build an MVP safely, ask one focused question. Otherwise proceed with reasonable assumptions and document them.

## Workflow

### 1. Repository Setup

1. Check whether Git is initialized.
2. If not initialized, use `git_manager` if available; otherwise run `git init -b main`.
3. Do not commit until final report stage.

### 2. Research And Planning

Use parallel subagents only when the user request explicitly authorizes orchestration or subagents and tools are available. Cap each research agent at 5 sources.

- Launch 2 `researcher` agents for idea validation, risks, constraints, and best solution shape.
- Launch 2 `researcher` agents for best-fit tech stack.
- Launch 2 `researcher` agents for design style, trends, fonts, colors, spacing, layout, and asset descriptions.
- Keep each research report under 150 lines with citations.
- If subagents are unavailable, run the same research locally with tight source limits.

Then:

1. Use `ui_ux_designer` to create or update `docs/design-guidelines.md`.
2. Create HTML wireframes under `docs/wireframe/` or `docs/wireframes/` using clear developer annotations.
3. If no logo is provided and visual identity matters, generate one with available image-generation tools.
4. Capture wireframe screenshots with available browser/screenshot tooling and save under `docs/wireframes/` when feasible.
5. Use `planner` to create a progressive disclosure implementation plan under `plans/`.

Plan structure:

- `plans/YYYYMMDD-HHmm-plan-name/plan.md`: overview under 80 lines with phase links and status.
- `phase-XX-phase-name.md`: Context links, Overview, Key Insights, Requirements, Architecture, Related code files, Implementation Steps, Todo list, Success Criteria, Risk Assessment, Security Considerations, Next steps.

### 3. Implementation

1. Implement the plan step by step in the main agent unless phase ownership requires subagents.
2. Use `fullstack_developer` for phase files with explicit ownership boundaries when useful.
3. Use `ui_ux_designer` for frontend/UI implementation that must follow `docs/design-guidelines.md`.
4. Generate, inspect, crop, resize, or edit assets only when needed.
5. Run typecheck, lint, compile, and build commands discovered from the project.
6. Never fake data or weaken checks just to pass build/tests.

### 4. Testing And Debugging

1. Write real tests for implemented behavior and edge cases.
2. Use `tester` when available to run tests and verify the app.
3. If tests fail, use `debugger` when available for root-cause analysis.
4. Fix issues in the main agent or assigned implementation agent.
5. Repeat until tests pass or blockers are clearly documented.

### 5. Code Review

1. Use `code_reviewer` for review after implementation.
2. Fix critical and high-severity issues.
3. Re-run relevant tests after fixes.
4. Document any accepted residual risk.

### 6. Documentation

Use `docs_manager` or docs skills to create or update:

- `README.md` or `docs/README.md`, under 300 lines.
- `docs/project-overview-pdr.md`.
- `docs/codebase-summary.md`.
- `docs/code-standards.md`.
- `docs/system-architecture.md`.
- `docs/project-roadmap.md` using `project_manager` when available.

Keep `docs/` as the source of truth.

### 7. Final Report And Commit

1. Summarize implemented scope, files changed, validations, test status, review status, docs updated, and unresolved questions.
2. Use `git_manager` to create conventional commits for all implemented changes.
3. Do not push unless explicitly requested.

### 8. Onboarding

Guide setup one question at a time. Ask for required API keys, provider choices, environment values, or configuration decisions only when needed. Wait for each answer before the next question.

## Output

Final response should include:

- What was built.
- Key decisions and trade-offs.
- Commands run and results.
- Tests/review/docs status.
- Commit hash if committed.
- How the user starts/configures the project.
- Unresolved questions.
