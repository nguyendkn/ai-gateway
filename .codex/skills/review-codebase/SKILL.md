---
name: review-codebase
description: Scan, research, and analyze a codebase with optional researcher, scout, code-reviewer, and planner agents. Use when the user asks to review a codebase, analyze architecture, code quality, security, technical debt, duplicate code, vulnerabilities, or create an improvement plan. Primarily analysis and planning; only edit code when explicitly requested.
---

# Review Codebase

Use this skill to run a disciplined codebase scan and produce actionable findings or an improvement plan. Default to analysis only. Do not modify product code, run destructive commands, commit, or push unless the user explicitly requests that action.

## Operating Rules

- Treat the user's `[tasks-or-prompt]` as the source of scope.
- Activate only the relevant skills and agents available in the current session.
- Apply YAGNI, KISS, and DRY in findings and recommendations.
- Prefer local evidence: docs, manifests, tests, source files, logs, and git state.
- Keep reports concise. Sacrifice grammar for concision when useful.
- List unresolved questions at the end of reports, only when real.
- If the request involves current external practices, libraries, standards, security guidance, or citations, browse or use researcher agents with primary/authoritative sources.
- Use visual asset generation or image analysis only when the review includes UI screenshots, design assets, diagrams, or generated images. Verify generated assets before recommending or using them.

## Workflow

1. Scope the review:
   - Identify requested focus: architecture, security, performance, duplicate code, test quality, maintainability, dependency risk, or full codebase.
   - Check `git status --short` and avoid reverting or overwriting unrelated user changes.
   - Read project docs first when present: `docs/codebase-summary.md`, `docs/code-standards.md`, `docs/system-architecture.md`, `docs/project-overview-pdr.md`, `README.md`, and workflow rules.

2. Research when needed:
   - Use at most 2 `researcher` agents in parallel if agent tooling is available.
   - Assign distinct scopes, such as "best practices/challenges" and "security/current library guidance".
   - Limit each researcher to max 5 sources/tool calls.
   - Keep each research report at or below 150 lines and include citations.
   - If researcher agents are unavailable, do targeted research directly and cite sources.

3. Scout the codebase:
   - Use `/scout:ext` when available; use `/scout` as fallback.
   - If slash commands are unavailable, use `rg`, `rg --files`, manifests, docs, and targeted reads.
   - Save concise scout notes under the active plan/report directory when one exists.
   - Map major domains, entry points, shared utilities, data flows, tests, and risk-heavy files.

4. Review code:
   - Use multiple `code-reviewer` agents in parallel only when agent tooling is available and scopes are independent.
   - Otherwise review locally with a code-review stance.
   - Prioritize findings by severity:
     - Critical: exploitable security issues, data loss, broken builds, production-breaking defects.
     - High: correctness bugs, auth/permission mistakes, significant performance issues, type unsafety.
     - Medium: maintainability risks, duplicate code, weak error handling, missing meaningful tests.
     - Low: minor style or cleanup suggestions.
   - Ground findings in file and line references.

5. Fix and test only when requested:
   - If the user asked for fixes, implement them with narrow edits and run relevant validation.
   - Do not use fake data, mocked success, skipped tests, or placeholder assertions to make checks pass.
   - Use `tester` and `debugger` agents if available for test/fail/fix loops.
   - Repeat until checks pass or a real blocker remains.

6. Create an improvement plan when requested or clearly useful:
   - Use `planner` agent if available; otherwise write the plan directly.
   - Create `plans/YYYYMMDD-HHmm-plan-name/`.
   - Save `plan.md` under 80 lines with phase links and status/progress.
   - Add phase files named `phase-XX-phase-name.md`.
   - Each phase file must include: Context links, Overview, Key Insights, Requirements, Architecture, Related code files, Implementation Steps, Todo list, Success Criteria, Risk Assessment, Security Considerations, Next steps.

7. Final report:
   - Lead with highest-severity findings.
   - Include scope, files reviewed, validation run, plan path if created, and next steps.
   - If no issues are found, say so and note residual risk or test gaps.
   - Ask whether to commit/push only if files changed and the user has not already requested git actions.
