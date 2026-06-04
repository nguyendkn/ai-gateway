---
name: test
description: Run, write, debug, or plan unit, integration, e2e, UI, type, lint, build, and regression tests.
---

# Test

Use this skill for validation and test work.

## Workflow

1. Identify the behavior or change to validate.
2. Discover test/build commands from manifests, Makefiles, docs, or CI.
3. Run targeted checks first.
4. Add or update tests when behavior changed.
5. Debug failures to root cause and rerun relevant checks.

## Routing

- Use `test-engineer` for strategy or test design.
- Use `tester` for execution-focused validation.
- Use `qa` or `qa-only` for browser/UI testing when available.

## Rules

- Do not weaken tests to pass.
- Do not use fake behavior just to satisfy checks.
- Report commands and results.
