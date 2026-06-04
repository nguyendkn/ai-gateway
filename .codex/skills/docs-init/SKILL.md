---
name: docs-init
description: Initialize repository documentation by analyzing an existing codebase and writing or refreshing docs/project-overview-pdr.md, docs/codebase-summary.md, docs/code-standards.md, docs/system-architecture.md, and README.md. Use when the user asks to create initial docs, bootstrap documentation, generate a PDR, summarize a codebase, establish code standards, document system architecture, or run a docs-manager/docs-init workflow without implementing product code.
---

# Docs Init

## Operating Rules

- Treat `docs/` as the source of truth for project documentation.
- Do not implement product code, change runtime behavior, or create features while using this skill. Limit edits to documentation files, `README.md`, and analysis artifacts such as `repomix-output.xml`.
- Analyze the actual repository before writing. Do not invent frameworks, commands, services, or test coverage.
- If the user explicitly asks for a `docs-manager` agent, subagent, or delegation and multi-agent tools are available, delegate a bounded documentation-analysis pass. Keep final review and file integration local.
- Prefer `rg`, `rg --files`, `find`, package manifests, config files, existing docs, and `git log` for repository discovery.
- Run `repomix` from the repository root when available to produce `repomix-output.xml`, then use it as an input for `docs/codebase-summary.md`. If `repomix` is unavailable or fails, continue with direct codebase inspection and note the gap.

## Workflow

1. Scan the repository structure:
   - Inspect `README.md`, `docs/`, source directories, tests, package manifests, build files, CI/deployment config, and environment examples.
   - Check commit history for conventions with `git log --oneline -n 20` when commits exist.
   - Identify the current implementation state, including empty or scaffold-only directories.

2. Generate or refresh analysis input:
   - Prefer `repomix --output repomix-output.xml` from the repo root.
   - Summarize from `repomix-output.xml` when it exists; otherwise summarize from inspected files.
   - Avoid copying secrets or large generated content into docs.

3. Create or update the core documentation set:
   - `docs/project-overview-pdr.md`: product overview, goals, non-goals, users, functional requirements, non-functional requirements, assumptions, constraints, acceptance criteria, and open questions.
   - `docs/codebase-summary.md`: current codebase state, directory map, key modules, dependencies, entry points, tests, tooling, gaps, and maintenance notes.
   - `docs/code-standards.md`: repository structure standards, naming, formatting, linting, testing, error handling, configuration, security, and documentation expectations.
   - `docs/system-architecture.md`: system context, components, data/control flow, external integrations, runtime/deployment model, configuration, observability, and architectural risks.
   - `README.md`: concise project entry point under 300 lines with status, setup or tooling notes, and links into `docs/`.

4. Keep documentation accurate and actionable:
   - State "not configured" or "not present" when tooling, tests, APIs, or deployment files are missing.
   - Use relative links and consistent Markdown headings.
   - Include commands only if they exist or are clearly marked as planned conventions.
   - Prefer short tables or bullets for scanability.

## Final Response

Report:

- Files created or updated.
- Commands run, including whether `repomix` succeeded.
- Current documentation gaps or assumptions.
- Any follow-up documentation work that should happen after implementation begins.
