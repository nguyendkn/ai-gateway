---
name: plan-fast
description: Create an implementation plan without external research or code changes. Use when Codex is asked to plan fast, analyze a task, create a plans/YYYYMMDD-HHmm-* directory, write plan.md and phase files, use planner-style analysis, and stop for user review before implementation.
---

# Plan Fast

## Mission

Analyze the requested task and create a concise implementation plan only. Do not implement, edit product code, run migrations, commit, or push.

## Operating Rules

- No external research. Use local repository evidence only.
- Activate `planning` skill if available. If unavailable, use the `planner` agent when available and follow this skill's plan structure.
- Activate other relevant local skills only for analysis, not implementation.
- Use the user's task as source of truth.
- Keep reports concise; sacrifice grammar for concision.
- List unresolved questions at the end of reports.
- Ask the user to review the plan before any implementation starts.

## Workflow

1. Parse `[task]`.
2. Create a plan directory:

```text
plans/YYYYMMDD-HHmm-plan-name
```

Use current local time from a shell command. Use a short kebab-case plan name derived from the task.

3. Pass the plan directory path to every subagent used.
4. Analyze the local skills catalog and choose needed analysis skills.
5. Read project docs when present:

```text
docs/codebase-summary.md
docs/code-standards.md
docs/system-architecture.md
docs/project-overview-pdr.md
```

If a required doc is missing, note it in `reports/01-context-report.md`; continue with available local evidence.

6. Inspect only relevant local source, tests, manifests, configs, and existing plans needed to plan accurately.
7. Use `planner` subagent when available to gather evidence and draft the plan. If subagents are unavailable, perform the same steps locally.
8. Create all plan files. Do not implement.
9. Ask user to review the plan.

## Directory Structure

```text
plans/
└── YYYYMMDD-HHmm-plan-name/
    ├── reports/
    │   ├── 01-context-report.md
    │   └── ...
    ├── plan.md
    ├── phase-01-phase-name-here.md
    └── ...
```

## Plan File

Save the overview access point at `plans/YYYYMMDD-HHmm-plan-name/plan.md`.

Rules:

- Keep under 80 lines.
- Keep generic and scannable.
- List each phase with status, progress, and links to phase files.
- Include links to reports.
- End with review prompt / approval gate.

Suggested structure:

```markdown
# {Plan Title}

## Summary
## Scope
## Phase Overview
| Phase | Status | Progress | File |
## Reports
## Risks
## Approval Gate
```

## Phase Files

Create one file per phase:

```text
plans/YYYYMMDD-HHmm-plan-name/phase-XX-phase-name-here.md
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

## Reports

Write concise supporting reports under `reports/` when useful. Always create `reports/01-context-report.md` with:

- Task summary.
- Skills and agents considered.
- Docs read and docs missing.
- Relevant local files inspected.
- Key findings.
- Unresolved questions, if any.

## Output

Return:

- Plan directory path.
- `plan.md` path.
- Phase count.
- Highest risks.
- Open questions.
- Clear statement that implementation has not started.
