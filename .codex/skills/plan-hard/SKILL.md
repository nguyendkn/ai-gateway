---
name: plan-hard
description: Research, analyze, and create a detailed implementation plan without implementing. Use when Codex is asked to "plan hard", research a task, create a plans/YYYYMMDD-HHmm-* directory with research/scout/reports, produce plan.md plus phase files, and ask the user to review before coding.
---

# Plan Hard

Use this skill for non-trivial planning that needs external research, repo analysis, and a structured implementation plan. Do not implement.

## Rules

- Treat `[task]` as the source of truth.
- Activate planning-style skills if available; otherwise use the `planner` agent and this skill's structure.
- Use max 2 researcher agents in parallel. Each covers a different aspect and should use max 5 tool calls.
- Pass the plan directory path to every subagent or helper used.
- Keep research reports <=150 lines with citations.
- Sacrifice grammar for concision in reports. Put unresolved questions at the end.
- Do not edit product code, tests, runtime config, migrations, or deployment files.
- Do not commit or push.
- Ask the user to review the plan before implementation starts.

## Workflow

### 1. Create Plan Directory

- Get current local time with a shell command.
- Create:

```text
plans/YYYYMMDD-HHmm-plan-name/
```

- Use a short kebab-case plan name derived from the task.
- Create subdirectories:

```text
research/
reports/
scout/
```

### 2. Research

- Use up to 2 `researcher` agents when available; otherwise research locally with the same limits.
- Split topics by aspect, for example:
  - product/API/runtime behavior.
  - library/provider/security/architecture options.
- Save reports as:

```text
research/researcher-01-report.md
research/researcher-02-report.md
```

- Include sources/citations. Prefer official docs and primary sources.

### 3. Repository Analysis

- Read these docs when present:

```text
docs/codebase-summary.md
docs/code-standards.md
docs/system-architecture.md
docs/project-overview-pdr.md
```

- If `docs/codebase-summary.md` is missing or older than 3 days, use scout-style local code search to find relevant files. Prefer GitNexus query/context tools when available; otherwise use `rg`/`rg --files`.
- Save scout notes under:

```text
scout/scout-01-report.md
```

- Always write a context report:

```text
reports/01-context-report.md
```

Include task summary, skills/agents used, docs read/missing, files inspected, key findings, assumptions, unresolved questions.

### 4. Plan Creation

- Give the planner agent, when available:
  - task.
  - plan directory path.
  - research report paths.
  - scout report paths.
  - context report path.
  - required file structure and phase section order.
- If planner agent is unavailable, create the plan locally.

### 5. Required Directory Structure

```text
plans/
└── YYYYMMDD-HHmm-plan-name/
    ├── research/
    │   ├── researcher-01-report.md
    │   └── researcher-02-report.md
    ├── reports/
    │   └── 01-context-report.md
    ├── scout/
    │   └── scout-01-report.md
    ├── plan.md
    ├── phase-01-phase-name-here.md
    └── ...
```

### 6. plan.md Specification

- Save overview at `plans/YYYYMMDD-HHmm-plan-name/plan.md`.
- Keep under 80 lines.
- Keep generic and scannable.
- List each phase with status, progress, and links to phase files.
- Link research, scout, and report artifacts.
- End with an approval gate.

Suggested structure:

```markdown
# {Plan Title}

## Summary
## Scope
## Phase Overview
| Phase | Status | Progress | File |
## Evidence
## Risks
## Approval Gate
```

### 7. Phase File Specification

Create one phase file per phase:

```text
phase-XX-phase-name-here.md
```

Each phase file must contain these sections in order:

```markdown
# Phase XX: {Phase Name}

## Context links
## Overview
## Key Insights
## Requirements
## Architecture
## Related code files
## Implementation Steps
## Todo list
## Success Criteria
## Risk Assessment
## Security Considerations
## Next steps
```

`Overview` must include date, description, priority, implementation status, and review status.

## Output

Return:

- Plan directory path.
- `plan.md` path.
- Research/scout/report paths.
- Phase count.
- Highest risks.
- Open questions.
- Clear statement: implementation has not started.
- Ask the user to review and approve the plan.
