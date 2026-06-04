---
name: docs-update
description: Analyze a repository and update documentation files without implementing product code. Use when the user asks for Docs Update, a docs-manager documentation refresh, README updates, PDR updates, codebase summaries, code standards, system architecture, project roadmap, deployment guide, design guidelines, or documentation synchronization based on the current codebase and additional requests.
---

# Docs Update

## Operating Rules

- Treat `docs/` as the source of truth for project documentation.
- Do not implement product code, change runtime behavior, or create features while using this skill.
- Limit edits to documentation files, `README.md`, and documentation analysis artifacts when needed.
- Analyze the actual repository before updating docs. Do not invent frameworks, commands, deployment targets, APIs, tests, or architecture.
- If the user explicitly asks for a `docs-manager` agent, subagent, or delegation and multi-agent tools are available, delegate a bounded documentation-analysis pass. Keep final file edits and consistency review local.
- Preserve useful existing documentation. Update stale sections instead of replacing whole files when targeted edits are enough.

## Inputs

Capture any user-provided additional requests and apply them as constraints or focus areas. Examples:

- "focus on deployment"
- "include security gaps"
- "do not touch README"
- "only update roadmap"

If additional requests conflict with required files, follow the user's newest explicit instruction and report what was skipped.

## Workflow

1. Inspect the repository:
   - Read existing `README.md` and `docs/`.
   - Inspect source directories, tests, manifests, config files, CI/deployment files, environment examples, and recent commits when present.
   - Use `rg`, `rg --files`, `find`, and targeted file reads. Avoid unnecessary large-file reads.

2. Update required documentation:
   - `README.md`: keep under 300 lines; provide a concise entry point, status, setup or tooling notes, and links into `docs/`.
   - `docs/project-overview-pdr.md`: update product overview, goals, requirements, constraints, acceptance criteria, assumptions, and open questions.
   - `docs/codebase-summary.md`: update current implementation state, directory map, modules, dependencies, tooling, tests, and gaps.
   - `docs/code-standards.md`: update repository structure, naming, formatting, linting, testing, error handling, configuration, security, and documentation standards.
   - `docs/system-architecture.md`: update system context, components, data/control flow, integrations, runtime/deployment model, observability, and risks.
   - `docs/project-roadmap.md`: update phases, milestones, priorities, dependencies, risks, and next actions.

3. Update optional documentation when relevant:
   - `docs/deployment-guide.md`: update only when deployment, infrastructure, environment, release, or operations details exist or are requested.
   - `docs/design-guidelines.md`: update only when UI, UX, branding, frontend, or design-system details exist or are requested.

4. Maintain quality:
   - State "not configured", "not present", or "not yet implemented" when details are missing.
   - Use relative Markdown links and consistent headings.
   - Keep examples and commands accurate to files in the repo.
   - Keep README below 300 lines and avoid duplicating full docs content there.

## Final Response

Report:

- Files created or updated.
- Important sources inspected.
- Whether optional docs were updated or skipped, and why.
- Assumptions, gaps, or follow-up documentation tasks.
