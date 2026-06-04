---
name: docs-summarize
description: Summarize repository documentation and codebase state using docs/codebase-summary.md as the primary source. Use when the user asks for Docs Summarize, a docs-manager summary report, a focused documentation/codebase summary, a current-state assessment from docs, or an optional targeted scan without implementing product code.
---

# Docs Summarize

## Operating Rules

- Treat `docs/` as the source of truth.
- Use `docs/codebase-summary.md` as the primary input.
- Do not implement product code, change runtime behavior, or update documentation unless the user explicitly asks for edits. This workflow normally responds with a summary report only.
- Do not scan the entire codebase unless the user explicitly requests it.
- If the user explicitly asks for a `docs-manager` agent, subagent, or delegation and multi-agent tools are available, delegate a bounded documentation-analysis pass. Keep the final report local.

## Arguments

Interpret user-provided arguments as:

- Focused topics: default to `all`. Examples: `architecture`, `testing`, `security`, `API`, `deployment`, `gaps`.
- Should scan codebase: default to `false`. Treat only explicit truthy values such as `true`, `yes`, `scan`, or `1` as permission to inspect code beyond documentation.

If arguments are not provided, summarize all major topics from `docs/codebase-summary.md` without a broad code scan.

## Workflow

1. Read documentation:
   - Start with `docs/codebase-summary.md`.
   - Read related docs only when needed for the requested topics, commonly `docs/project-overview-pdr.md`, `docs/code-standards.md`, `docs/system-architecture.md`, and `README.md`.
   - If `docs/codebase-summary.md` is missing, report that the summary source is unavailable and use other docs only as fallback.

2. Scope any code inspection:
   - When `should_scan_codebase` is false, do not inspect the full codebase. It is acceptable to list top-level paths or check for the existence of files directly referenced by docs.
   - When `should_scan_codebase` is true, run a targeted scan for the requested topics using `rg`, `rg --files`, manifests, config files, and tests. Avoid broad, unfocused file reads.

3. Produce a summary report:
   - Current State Assessment: what the docs say exists today.
   - Focused Findings: answer the requested topics first; for `all`, cover product scope, architecture, modules, tooling, tests, security/configuration, and deployment.
   - Documentation Gaps: missing, stale, or ambiguous docs.
   - Codebase Verification: state whether code was scanned, and summarize what was verified.
   - Recommendations: short prioritized actions for documentation or future implementation.

## Output Style

- Keep the report concise and evidence-based.
- Clearly distinguish facts from documentation, code observations, and assumptions.
- Mention missing docs or unavailable files directly instead of inferring details.
- Include file references for important sources when useful.
