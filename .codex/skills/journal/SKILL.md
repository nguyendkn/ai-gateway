---
name: journal
description: Record technical incidents, repeated failures, hard lessons, blockers, production issues, and postmortem-style engineering notes.
---

# Journal

Use this skill to create a concise engineering journal entry.

## Workflow

1. Determine event, date, severity, component, and status.
2. Capture what happened, technical details, attempted fixes, root cause, lessons, and next steps.
3. Write to `docs/journals/YYMMDDHHmm-title.md` only when persistence is requested.

## Rules

- Use `journal-writer` when delegation is helpful.
- Be factual and direct.
- Redact secrets and private data.
- Do not invent missing evidence.
