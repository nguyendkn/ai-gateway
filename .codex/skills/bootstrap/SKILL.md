---
name: bootstrap
description: Bootstrap a new software project step by step from user requirements with explicit approval gates for Git initialization, tech stack, implementation plan, optional design/wireframes, implementation, testing, review, documentation, onboarding, and optional commit or push. Use when Codex is asked to run a careful staged project bootstrap, challenge assumptions, compare alternatives, use research/planning/design/testing/review subagents when available and allowed, and avoid implementing before approval.
---

# Bootstrap

## Overview

Bootstrap a new project through staged discovery, research, stack choice, planning, optional design, implementation, testing, review, docs, onboarding, and final git handoff.

Use YAGNI, KISS, DRY. Be blunt about feasibility, tradeoffs, and overengineering.

## Required Reference

Read `references/bootstrap-protocol.md` before starting work.

## Operating Rules

- Treat user requirements as source of truth.
- Read top-level guidance first: `AGENTS.md`, `.codex/config.toml`, and relevant `docs/` when present.
- First check if Git is initialized. If not, ask user before initializing.
- Ask one blocking question at a time.
- Analyze available skills and activate only those needed.
- Use subagents only when available and allowed by current tool policy; otherwise perform same work locally and state fallback.
- Keep research reports under 150 lines with citations.
- Sacrifice grammar for concision in reports.
- Do not implement before the user approves the plan.
- Do not fake data, weaken tests, or ignore failures.
- Do not expose or commit secrets.
- Ask before commit or push.

## Output Style

- Concise.
- Decision-first.
- Pros/cons when choosing architecture or stack.
- Unresolved questions at end of reports.

## Completion

End with summary, validations, docs status, setup steps, unresolved questions, and whether user wants git commit/push.
