---
name: bootstrap-auto
description: Bootstrap new software projects end-to-end from user requirements. Use when Codex is asked to create or scaffold a new project, research feasibility, choose a tech stack, design UX, implement features, test, review, document, onboard the user, and optionally commit or push the result.
---

# Bootstrap Auto

Use this skill to turn a project idea into a working, documented, tested repository. Keep execution pragmatic: YAGNI, KISS, DRY. Be direct about feasibility, risks, and over-engineering.

## Operating Rules

- Read top-level guidance first: `AGENTS.md`, `CLAUDE.md`, `.codex/config.toml`, and relevant `docs/` files when present.
- Activate relevant skills and use available project agents when they exist: `researcher`, `planner`, `brainstormer`, `ui-ux-designer`, `fullstack-developer`, `tester`, `debugger`, `code-reviewer`, `docs-manager`, `project-manager`, and `git-manager`.
- If an expected agent or skill is unavailable, continue with the closest local capability and state the fallback.
- Keep generated research and handoff reports concise. Target <=150 lines unless the user asks for more.
- Sacrifice grammar for concision in reports. Put unresolved questions at the end.
- Do not use fake data merely to pass tests. Do not ignore failing tests.
- Do not commit secrets, API keys, provider tokens, customer data, or local `.env` files.

## Workflow

### 1. Git Baseline

- Check `git status` and whether `.git/` exists.
- If Git is not initialized, initialize it on branch `main`. Prefer `git-manager` when available; otherwise run the smallest safe git commands yourself.
- Do not overwrite unrelated user changes.

### 2. Requirements And Feasibility

- Parse the user's requirements into goals, non-goals, constraints, risks, and acceptance criteria.
- Ask only blocking questions. Otherwise make explicit assumptions and proceed.
- Use researcher/planner-style agents or web research for non-trivial product, architecture, security, or stack decisions when network is available.
- Summarize feasibility with hard tradeoffs, not generic optimism.

### 3. Tech Stack

- Compare 2-3 realistic stack options against project goals, team constraints, deployment model, security, cost, and maintainability.
- Choose the simplest stack that satisfies the requirements.
- Write the selected stack and rationale under `docs/`, usually `docs/tech-stack.md`.

### 4. Product Plan

- Create `plans/YYYYMMDD-HHmm-plan-name/`.
- Add `plan.md` as the overview access point, under 80 lines, with phases, status/progress, links, risks, and approval gates.
- Add phase files named `phase-XX-phase-name.md` when the project needs progressive disclosure.
- Include, when relevant: context links, overview, key insights, requirements, architecture, related files, implementation steps, todos, success criteria, risk assessment, security considerations, next steps.

### 5. Design And Wireframes

- Create `docs/design-guidelines.md` when the project has a user-facing UI.
- Generate developer-ready HTML wireframes under `docs/wireframe/` or `docs/wireframes/`; keep naming consistent with existing repo docs.
- Use real or generated visual assets when the product needs them. If no logo exists and branding matters, use image-generation capability when available.
- Verify wireframes visually when browser tooling is available; save screenshots under `docs/wireframes/`.
- Stop for user review when visual direction materially affects implementation. Iterate until approved.

### 6. Implementation

- Implement phase by phase from the plan.
- Prefer existing repo patterns, scripts, framework conventions, and checked-in commands.
- Keep edits scoped to the active phase and acceptance criteria.
- Run typecheck/compile/static checks as soon as commands are discoverable.
- For UI work, follow `docs/design-guidelines.md` and verify responsive behavior.

### 7. Testing Loop

- Write or update tests for implemented behavior before declaring completion.
- Use `tester` when available to run relevant unit, integration, e2e, coverage, lint, and build checks.
- If tests fail, use `debugger` when available to identify root cause, then fix and rerun.
- Repeat until tests pass, a clear external blocker remains, or the user changes scope.

### 8. Code Review Loop

- Use `code-reviewer` when available after implementation and testing.
- Fix critical/high findings, then rerun relevant tests.
- Repeat until no blocking findings remain or unresolved risks are explicitly documented.

### 9. Documentation And Roadmap

- Use `docs-manager` when available to create or update:
  - `docs/README.md` or root `README.md` as appropriate.
  - `docs/project-overview-pdr.md`.
  - `docs/code-standards.md`.
  - `docs/system-architecture.md`.
  - API, deployment, runbook, or configuration docs when relevant.
- Use `project-manager` when available to create or update `docs/project-roadmap.md`.
- Record history logs or lesson-learn entries when local repo instructions require them.

### 10. Onboarding

- Guide the user through setup one question at a time.
- Ask for secrets or API keys only when needed, and instruct the user to put them in local env files or secret stores. Do not echo or commit secrets.
- Repeat configuration review until the user approves.

### 11. Final Report

- Summarize what was built, files changed, validation run, remaining risks, setup steps, and next actions.
- Ask whether the user wants to commit and push. If yes, use `git-manager` when available.

## Success Criteria

- Repository is initialized and organized.
- Requirements, stack, plan, design, implementation, tests, docs, and onboarding are aligned.
- Validation commands pass or blockers are explicit.
- No fake test data, ignored failures, or committed secrets.
- Next steps are concrete and prioritized.
