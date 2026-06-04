---
name: plan-cro
description: Create concise conversion rate optimization plans for landing pages, funnels, ads, forms, checkout flows, screenshots, videos, URLs, or user-provided CRO issues. Use when Codex is asked to analyze conversion problems, audit above-fold copy, CTA psychology, form friction, message match, social proof, urgency, pricing, objections, mobile UX, tracking, or produce a CRO plan without implementing it.
---

# Plan CRO

## Overview

Create CRO planning artifacts from issues, URLs, screenshots, videos, or local product/page code. Keep reports concise, actionable, and approval-gated.

## Workflow

1. Identify input type: raw issues, URL, screenshot, video, document, or codebase/page target.
2. Activate needed skills/tools from available catalog:
   - Use multimodal/image/video/document analysis skills for screenshots, videos, PDFs, or visual artifacts.
   - Use URL fetch/browser tools for URLs. Cite sources.
   - Use `/scout:ext` when available for code discovery; fallback to `/scout`, GitNexus, `rg`, or local inspection.
   - Use planner/planning agent or skill when available; fallback to direct plan writing.
3. Load `references/cro-framework.md` before analysis.
4. Describe visual issues in enough detail that a copywriter/designer can act without seeing the original.
5. Analyze issues with the CRO framework, prioritizing conversion impact over polish.
6. Create plan files only. Do not implement until user approves.

## Plan Output

Create directory:

```text
plans/YYYYMMDD-HHmm-plan-name/
```

Use local timezone from runtime context. Make `plan-name` short, lowercase, hyphen-case.

Create:

```text
plan.md
phase-01-phase-name.md
phase-02-phase-name.md
...
```

`plan.md` rules:

- Under 80 lines.
- Generic overview access point.
- List each phase with status/progress and links.
- Include source inputs, assumptions, and unresolved questions summary.

Each phase file sections:

- Context links
- Overview with date, priority, status
- Key insights
- Requirements
- Architecture or page/funnel area
- Related code files, if any
- Implementation steps
- Todo list
- Success criteria
- Risk assessment
- Security considerations
- Next steps
- Unresolved questions

Research markdown reports:

- Max 150 lines each.
- Include concise citations when external sources are used.
- Prefer bullets, fragments, short lines. Sacrifice grammar for concision.

## CRO Analysis Rules

- Start with offer, audience, traffic intent, and conversion goal.
- Use PAS: Problem, Agitate, Solve.
- Score above-fold clarity, CTA, form friction, trust, proof, objections, mobile, speed/tracking.
- Prefer specific copy and layout changes over vague advice.
- Separate quick wins from structural tests.
- Keep tests one-variable where possible.
- Mark urgency as valid only when real.
- Put social proof and trust signals near decision points.
- Use first-person CTA copy when useful.
- Keep forms to 5 fields max unless there is a stated business reason.
- Match TOFU/MOFU/BOFU intent.
- Include tracking events needed to judge lift.

## Approval Gate

End with: plan created, path, top risks, unresolved questions. Wait for user approval before implementation.
