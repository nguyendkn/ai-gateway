---
name: plan-ci
description: Analyze GitHub Actions logs and produce a fix plan without implementing. Use when Codex is given a GitHub Actions run, job, workflow, or failure URL and must inspect CI logs, identify root causes, compare implementation approaches, recommend one, and ask for confirmation before code changes.
---

# Plan CI

Use this skill to diagnose GitHub Actions failures and produce an implementation plan. Do not implement fixes until the user confirms.

## Rules

- Treat `[github-actions-url]` as the primary input.
- Activate relevant local skills and agents: `planner`, `debugger`, `tester`, GitHub/CI skills, and GitNexus skills when useful.
- Use the `planner` agent when available to structure the fix plan. Use `debugger` when log/root-cause tracing is complex.
- Prefer `gh` for GitHub Actions logs when authenticated. Use browser/web access only when needed and allowed.
- Inspect local workflow files, manifests, lockfiles, test configs, scripts, and related source after identifying the failing step.
- Do not change code, config, tests, or workflow files during this skill.
- Ask user for confirmation before implementation.
- Keep outputs concise. Sacrifice grammar for concision. List unresolved questions at the end.

## Workflow

### 1. Parse The CI Target

- Extract repository, workflow run id, job id/name, branch, commit SHA, and failing URL type when possible.
- If URL is missing or inaccessible, ask for the URL or pasted logs.
- Check `gh auth status` only if needed.

### 2. Collect Logs

- Use `gh run view`, `gh run view --log`, `gh run view --job`, or equivalent commands when available.
- Capture:
  - Failed workflow/job/step names.
  - Error messages and stack traces.
  - Relevant command output before failure.
  - Runner OS, dependency install step, cache step, service containers, env hints.
- Avoid dumping full logs into the response; quote only concise excerpts.

### 3. Inspect Local Evidence

- Read relevant local files:
  - `.github/workflows/*`.
  - `Makefile`, package scripts, build/test configs.
  - Language manifests and lockfiles.
  - Source/tests mentioned in the failure.
- Use `rg`/`rg --files` for search.
- For this repo, follow investigation-first and harness architecture rules for regressions or failure-class fixes.

### 4. Root Cause

- Identify the most likely root cause and evidence.
- If not definitive, rank likely causes and name missing evidence.
- Distinguish CI environment issues, dependency issues, test failures, lint/type errors, secrets/env gaps, flaky tests, service startup issues, permission problems, and workflow syntax/config issues.

### 5. Fix Plan

- Provide at least 2 implementation approaches.
- For each approach include:
  - What changes.
  - Pros.
  - Cons.
  - Risk.
  - Validation commands.
- Recommend one approach with rationale.
- Include expected files to change and tests/checks to run.
- End by asking the user to confirm before implementation.

## Output Shape

- Summary: failing workflow/job/step and impact.
- Evidence: concise log excerpts and local files inspected.
- Root Cause: confirmed or likely.
- Approaches: at least 2 with trade-offs.
- Recommendation: chosen approach and why.
- Implementation Plan: ordered steps, expected files, validation.
- Confirmation: ask before implementing.
- Unresolved Questions: remaining unknowns, if any.
