---
name: edge-case-review-parallel
description: Exhaustively identify edge cases, then verify them with parallel code-reviewer agents without fixing by default. Use when Codex is asked to review a codebase, module, feature, PR, or scope for edge cases, security gaps, error handling, race conditions, validation holes, resource leaks, or untested paths.
---

# Edge Case Review Parallel

Use this skill for review-only edge-case verification. The main agent lists edge cases first; code-reviewer agents verify specific assigned cases. Do not fix unless user approves a follow-up fix.

## Rules

- Treat `[scope-or-prompt]` as source of truth.
- Activate relevant local skills and agents, especially `code-reviewer`, GitNexus skills, `debugger`, and `tester` when useful.
- Read `docs/codebase-summary.md` first when present.
- Use scout-style local discovery to find relevant files: prefer GitNexus query/context when available; otherwise use `rg` and `rg --files`.
- Keep reports concise. Sacrifice grammar for concision. List unresolved questions at the end.
- Do not modify product code, tests, configs, migrations, or docs during review.
- Do not commit or push during review.
- Ask before triggering any fix pipeline or commit.

## Workflow

### 1. Identify Edge Cases First

The main agent must list potential edge cases before launching reviewers.

Think across:

- Null, nil, undefined, missing, empty, malformed values.
- Boundary conditions: zero, one, max, min, off-by-one, overflows, truncation.
- Error handling: provider errors, partial failures, retries, cancellation, timeouts.
- Async/race issues: concurrent requests, ordering, stale state, duplicate events.
- Input validation: untrusted input, type confusion, encoding, large payloads.
- Security: authn/authz, tenant isolation, injection, SSRF, secret leakage, unsafe logs.
- Resource lifecycle: leaks, unclosed files/bodies, goroutines/tasks, DB transactions.
- Persistence: transaction boundaries, idempotency, migrations, uniqueness, consistency.
- Observability: missing trace/error context, misleading metrics, alert gaps.
- Untested paths: branches, feature flags, fallback paths, mocks hiding behavior.

Output edge cases in this shape:

```markdown
## Edge Cases Identified

### Category: {scope-area}
1. {edge case description} -> files: {file1, file2}
2. {edge case description} -> files: {file3}
```

### 2. Categorize And Assign

- Group edge cases by similar scope.
- Use max 6 categories. Merge small categories.
- Assign each category to one `code-reviewer` agent when available.
- Each reviewer must verify assigned cases, not discover new broad areas.
- If subagents are unavailable, perform the same verification locally category by category.

Reviewer prompt pattern:

```text
Verify these specific edge cases in the given files:
{edge cases}

Relevant files:
{files}

For each, report:
- Handled: how it is handled
- Unhandled: what is missing
- Partial: what needs improvement

Do not modify files.
```

### 3. Parallel Verification

- Launch all category reviewers in parallel when tools allow it.
- Pass only category name, edge cases, relevant files, and review-only instruction.
- Ask reviewers for concise findings with file/line references when possible.
- Wait for all reports before aggregating.

### 4. Aggregate Results

Create final report:

```markdown
## Edge Case Verification Report

### Summary
- Total edge cases:
- Handled:
- Unhandled:
- Partial:

### Unhandled Edge Cases
| # | Edge Case | File | Status |
|---|-----------|------|--------|

### Partial Handling
| # | Edge Case | File | Issue |
|---|-----------|------|-------|

### Handled Edge Cases
| # | Edge Case | Evidence |
|---|-----------|----------|

### Unresolved Questions
```

### 5. Fix Gate

- If unhandled or partial cases exist, ask:

```text
Found N unhandled/partial edge cases. Fix them with parallel implementation? [Y/n]
```

- If yes, use available fix/parallel implementation workflow such as `parallel-implement`, `cook-auto`, or local implementation.
- If no, stop after the report.

### 6. Commit Gate

- After any approved fixes and validation, ask:

```text
Commit? [Y/n]
```

- If yes, use `git-manager` or `git-commit`.
- Do not push unless explicitly requested.

## Success Criteria

- Edge cases are listed before verification starts.
- Reviewers verify assigned cases rather than discovering unrelated issues.
- Aggregate report distinguishes handled, unhandled, and partial cases.
- No code changes happen without explicit user approval.
