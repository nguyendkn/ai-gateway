# Bootstrap Protocol

Use this protocol for careful step-by-step project bootstrap.

## 1. Git Baseline

1. Check `.git/` and `git status --short`.
2. If Git not initialized, ask: "Initialize Git here?"
3. If yes, use `git-manager` when available; fallback `git init -b main`.
4. Never overwrite unrelated user changes.

## 2. Requirements

1. Capture `<user-requirements>` exactly.
2. Extract goals, non-goals, users, constraints, integrations, risks, acceptance criteria.
3. Ask one blocking question at a time.
4. If not blocked, proceed with explicit assumptions.
5. Challenge assumptions. Call out unrealistic scope, overengineering, hidden ops burden.

## 3. Research

Use multiple `researcher` subagents in parallel only when allowed. Otherwise research locally.

Research:

- Idea validation.
- Similar products / representative OSS implementations.
- Technical risks.
- Security/privacy/compliance issues.
- Deployment and cost constraints.
- Best-fit solution options.

Rules:

- Each research report <=150 lines.
- Include citations when web sources used.
- Prefer official docs and representative repos for technical choices.

## 4. Tech Stack

1. Ask user for preferred stack.
2. If user provides stack, skip stack search.
3. If not, compare 2-3 viable stacks with planner/researcher work.
4. Judge by simplicity, maintainability, deployment, team skill, cost, security, testability.
5. Recommend one stack. Include pros/cons.
6. Ask user to approve stack.
7. If user changes stack, repeat.
8. Write approved stack to `docs/tech-stack.md`.

## 5. Implementation Plan

Use planner-style work to create:

```text
plans/YYYYMMDD-HHmm-plan-name/
plan.md
phase-01-phase-name.md
phase-02-phase-name.md
...
```

`plan.md`:

- Under 80 lines.
- Overview access point.
- Phase list with status/progress and links.
- Pros/cons.
- Approval gates.
- Top risks.

Each phase file:

- Context links
- Overview with date, priority, status
- Key insights
- Requirements
- Architecture
- Related code files
- Implementation steps
- Todo list
- Success criteria
- Risk assessment
- Security considerations
- Next steps
- Unresolved questions

Stop after plan. Ask user to approve before implementation.

## 6. Wireframe And Design

Ask user: "Create wireframes and design guidelines?"

If no, skip to implementation after approved plan.

If yes:

1. Use `ui-ux-designer` and researcher-style work when available.
2. Research style, trends, fonts, colors, borders, spacing, element positions.
3. Predict likely Google Font and sizes from screenshots when supplied; do not default to Inter/Poppins without evidence.
4. Describe assets clearly enough for image generation.
5. Create `docs/design-guidelines.md`.
6. Create HTML wireframes in `docs/wireframe/` or `docs/wireframes/`; keep one convention.
7. If no logo and brand matters, generate logo with image tools.
8. Read/analyze generated assets with multimodal tools when available.
9. Use image tools for background removal, crop, resize, editing when needed.
10. Use browser/screenshot tooling to capture wireframes; save screenshots under `docs/wireframes/`.
11. Ask user to approve design.
12. If user requests changes, iterate until approved.

## 7. Implementation

Only start after approved plan and approved design when design was requested.

1. Implement phase by phase from `plans/`.
2. Main agent owns integration and final code quality.
3. Use frontend/design agents for UI work when available.
4. Use existing framework conventions and scripts.
5. Keep edits scoped to active phase.
6. Generate assets only when needed; verify generated assets.
7. Run typecheck/compile/build as commands become available.

## 8. Testing

1. Write real tests for behavior and edge cases.
2. Do not use fake data only to pass tests.
3. Use `tester` when available.
4. If tests fail, use `debugger` when available for root cause.
5. Fix, rerun, repeat.
6. Stop only when tests pass or blocker is explicit.

## 9. Code Review

1. Use `code-reviewer` when available after tests.
2. Fix critical/high issues.
3. Rerun relevant tests.
4. Repeat until no blocking issues remain.
5. Ask user to review and approve completed changes.

## 10. Documentation

After user approves changes, update docs when needed:

- `docs/README.md` under 300 lines.
- `docs/codebase-summary.md`.
- `docs/project-overview-pdr.md`.
- `docs/code-standards.md`.
- `docs/system-architecture.md`.
- `docs/project-roadmap.md`.

Use docs/project-manager agents or local docs skills when available.

## 11. Onboarding

Guide user setup step by step.

- Ask one configuration question at a time.
- Wait for answer before next.
- Use local env files or secret stores for secrets.
- Never echo or commit secrets.

## 12. Final Report

Report:

- What changed.
- Key decisions and tradeoffs.
- Files changed.
- Commands run and results.
- Tests/review/docs status.
- Setup guide.
- Next steps.
- Unresolved questions.

Ask if user wants commit and push. If yes, use `git-manager` when available.
