---
name: plan-two
description: Research a task and create an implementation plan with at least two approaches, clear trade-offs, pros and cons, and a recommended approach. Use when Codex is asked to plan two approaches, research before planning, create a plans/YYYYMMDD-HHmm-* directory, use researcher/scout/planner agents, and stop for user review before implementation.
---

# Plan Two

## Mission

Research the requested task, inspect the local codebase, and create a concise implementation plan with at least two viable approaches. Do not implement, edit product code, run migrations, commit, or push.

## Operating Rules

- Activate `planning` skill if available. If unavailable, follow this skill's plan structure and use `planner` agent when available.
- Analyze the local skills catalog and activate only skills needed for analysis.
- Use multiple `researcher` agents in parallel when available.
- Use `scout` agent when available to search the codebase. If no scout agent exists, perform codebase scout locally with `rg`, docs, manifests, configs, and targeted reads.
- Pass the plan directory path to every subagent.
- Limit each researcher to max 5 research tool calls / external sources.
- Prefer official docs, primary sources, release notes, standards, and representative open-source implementations.
- Keep reports concise; sacrifice grammar for concision.
- List unresolved questions at the end of reports.
- Ask the user to review the plan before any implementation starts.

## Workflow

1. Parse `[task]` and define planning scope, non-goals, and decision criteria.
2. Create a plan directory:

```text
plans/YYYYMMDD-HHmm-plan-name
```

Use current local time from a shell command. Use a short kebab-case plan name derived from the task.

3. Create `reports/` inside the plan directory.
4. Analyze available skills and agents. Record selected skills/agents in `reports/01-orchestration-report.md`.
5. Run parallel research:
   - Split research by distinct aspects, for example architecture, library/tooling, security, migration, testing, performance, UX, operations.
   - Save each research report as `reports/XX-research-{aspect}.md`.
   - Include sources, source dates when useful, key findings, risks, and unresolved questions.
   - Cap each researcher at max 5 research tool calls / external sources.
6. Run codebase scout:
   - Use scout agent if available.
   - Otherwise inspect local docs and code with `rg`, `rg --files`, targeted reads, manifests, configs, tests, and existing plans.
   - Save as `reports/XX-scout-report.md`.
7. Give the `planner` subagent:
   - Task text.
   - Plan directory path.
   - Research report file paths.
   - Scout report file path.
   - Required output structure from this skill.
8. Create `plan.md` and phase files in the plan directory.
9. Stop and ask user to review. Do not implement.

## Directory Structure

```text
plans/
└── YYYYMMDD-HHmm-plan-name/
    ├── reports/
    │   ├── 01-orchestration-report.md
    │   ├── 02-research-architecture.md
    │   ├── 03-research-tooling.md
    │   ├── 04-scout-report.md
    │   └── ...
    ├── plan.md
    ├── phase-01-phase-name-here.md
    └── ...
```

## Plan File

Save the overview access point at `plans/YYYYMMDD-HHmm-plan-name/plan.md`.

Rules:

- Keep under 100 lines unless the task is unusually large.
- Link every report and phase file.
- Include at least two implementation approaches.
- For each approach include scope, pros, cons, risks, validation, and migration/rollout notes.
- Recommend one approach with rationale.
- List each implementation phase with status, progress, and link to phase file.
- End with review prompt / approval gate.

Suggested structure:

```markdown
# {Plan Title}

## Summary
## Inputs
## Evidence
## Approaches
### Approach A: {Name}
### Approach B: {Name}
## Recommendation
## Phase Overview
| Phase | Status | Progress | File |
## Risks
## Open Questions
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

## Planner Prompt Requirements

When invoking `planner`, include:

- "Create at least 2 implementation approaches with clear trade-offs."
- "Explain pros and cons of each approach."
- "Provide a recommended approach and why."
- "Use the provided research and scout reports as evidence."
- "Follow the plan directory structure and phase file sections exactly."
- "Do not implement."

## Output

Return:

- Plan directory path.
- `plan.md` path.
- Research report paths.
- Scout report path.
- Phase count.
- Recommended approach.
- Highest risks.
- Open questions.
- Clear statement that implementation has not started.
