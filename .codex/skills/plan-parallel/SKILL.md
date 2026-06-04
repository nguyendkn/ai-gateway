---
name: plan-parallel
description: Create detailed implementation plans with dependency-aware, parallel-executable phases and exclusive file ownership. Use when the user asks for /vk:plan:parallel, a parallel implementation plan, phase dependency graph, file ownership matrix, or a plan optimized for fullstack-developer agents. Planning only; do not implement.
---

# Plan Parallel

## Mission

Create a comprehensive Markdown implementation plan optimized for parallel execution. Do not implement, edit product code, commit, or push.

## Operating Rules

- Treat `[task]` as source of truth.
- Activate planning skill if available; otherwise use `planner` agent and this workflow.
- Use relevant skills and agents only for research/planning.
- Keep reports concise; sacrifice grammar for concision.
- List unresolved questions at the end.
- Every implementation file must be owned by exactly one phase.
- If exclusive file ownership cannot be guaranteed, mark the phase sequential or ask for review.

## Workflow

1. Create the plan directory.
   - Prefer the naming pattern from injected `## Naming` context.
   - If no naming context exists, use `plans/YYYYMMDD-HHmm-task-slug/`.
   - Pass the plan directory path to every subagent.

2. Create required subdirectories:

```text
{plan-dir}/research/
{plan-dir}/reports/
{plan-dir}/scout/
```

3. Research in parallel when useful.
   - Use max 2 `researcher` agents.
   - Assign different aspects of the task.
   - Limit each agent to max 5 tool calls or sources.
   - Save reports under `{plan-dir}/research/`.
   - Keep each report at or under 150 lines with citations when external sources are used.

4. Analyze project docs:

```text
docs/codebase-summary.md
docs/code-standards.md
docs/system-architecture.md
docs/project-overview-pdr.md
```

If `docs/codebase-summary.md` is missing or older than 3 days:

- Use `/vk:scout <instructions>` if available.
- Otherwise emulate scout with `rg`, `rg --files`, manifests, docs, and targeted reads.
- Save scout outputs under `{plan-dir}/scout/`.

5. Use `planner` agent when available.
   - Pass task, plan dir, research report paths, scout report paths, relevant docs, and all requirements below.
   - If unavailable, create the plan locally.

6. Ask the user to review the plan.
   - Do not start implementation.

## Parallelization Requirements

The plan must create phases that:

- Are self-contained where possible.
- Have no overlapping file ownership.
- Separate concerns by architecture layer, feature domain, stack, directory, or test scope.
- Minimize coupling through explicit interfaces.
- Mark dependencies and sequential requirements clearly.

Preferred strategy:

- Split frontend, backend, database, infrastructure, and tests when ownership is clean.
- Isolate domains such as auth, profile, payments, admin, or analytics.
- Put integration/e2e tests after dependent implementation phases.

Example:

```text
Phase 01: Database Schema - parallel group A
Phase 02: Backend API Layer - parallel group A
Phase 03: Frontend Components - parallel group A
Phase 04: Integration Tests - depends on 01, 02, 03
```

## Required Plan Structure

```text
{plan-dir}/
├── research/
│   ├── researcher-01-report.md
│   └── ...
├── reports/
│   ├── 01-report.md
│   └── ...
├── scout/
│   ├── scout-01-report.md
│   └── ...
├── plan.md
├── phase-01-phase-name-here.md
└── ...
```

## plan.md Requirements

`{plan-dir}/plan.md` must start with YAML frontmatter:

```yaml
---
title: "{Brief title}"
description: "{One sentence for card preview}"
status: pending
priority: P2
effort: "{sum of phases, e.g., 4h}"
branch: "{current git branch}"
tags: [relevant, tags]
created: "{YYYY-MM-DD}"
---
```

Keep `plan.md` under 80 lines. Include:

- Summary.
- Scope and non-goals.
- Dependency graph.
- Execution strategy, for example `Phases 1-3 parallel, then Phase 4`.
- Phase table with status, progress, parallel group, and links.
- File ownership matrix.
- Reports and research links.
- Review / approval gate.

## Phase File Requirements

Each `{plan-dir}/phase-XX-phase-name-here.md` must contain these sections in order:

```markdown
# Phase XX: {Phase Name}

## Context links
## Parallelization Info
## Overview
## Key Insights
## Requirements
## Architecture
## Related code files
## File Ownership
## Implementation Steps
## Todo list
## Success Criteria
## Conflict Prevention
## Risk Assessment
## Security Considerations
## Next steps
```

Phase rules:

- `Parallelization Info`: state concurrent phases and dependencies.
- `Overview`: include date, description, priority, implementation status, review status.
- `Related code files`: list only files relevant to this phase.
- `File Ownership`: explicit files this phase may modify; no overlap with other phases.
- `Conflict Prevention`: explain how this phase avoids conflicts with parallel phases.

## Post-Plan Validation

Check injected `## Plan Context` for `Validation: mode=X, questions=MIN-MAX`.

- `prompt`: ask user whether to validate with brief interview.
- `auto`: run or emulate `/vk:plan:validate {plan-path}`.
- `off`: skip validation.

If no validation mode exists, ask the user to review the plan and stop.

## Output

Return:

- Plan directory path.
- `plan.md` path.
- Phase count.
- Parallel execution strategy.
- Highest risks.
- Open questions.
- Statement that implementation has not started.
