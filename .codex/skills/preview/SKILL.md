---
name: preview
description: Preview markdown, docs, generated pages, local apps, screenshots, or UI output using local server/browser tooling.
---

# Preview

Use this skill when the user wants to see, render, or verify an output visually.

## Workflow

1. Identify the artifact type: markdown, static HTML, app route, image, PDF, or generated asset.
2. Use the lightest preview path: direct file open, local server, browser QA, screenshot, or `view_image`.
3. Start a dev server only when the artifact requires one.
4. Verify the preview actually renders.
5. Report the local URL/path and any visual or runtime issues found.

## Rules

- Do not call legacy preview scripts.
- Stop any server you started unless the user needs it running.
- For web QA, use `browse`, `qa`, or `qa-only` when available.
