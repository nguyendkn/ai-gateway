---
name: integration-planning
description: Plan or implement third-party integrations such as payment providers, webhooks, APIs, SDKs, and external services.
---

# Integration Planning

Use this skill for external service integrations.

## Workflow

1. Identify provider, API surface, auth model, webhook requirements, and data model.
2. Inspect existing integration patterns in the repo.
3. Use official docs for current provider behavior when implementation depends on it.
4. Plan data flow, retries/idempotency, error handling, tests, and rollout.
5. Implement only after the integration contract is clear.

## Routing

- Use `dependency-expert` for SDK/package selection.
- Use `researcher` for official docs and provider references.
- Use `security-reviewer` for auth, secrets, webhook verification, and payment paths.

## Rules

- Do not hardcode secrets.
- Validate webhook signatures where supported.
- Design idempotency for payment and state-changing callbacks.
- Add tests or documented validation for critical paths.
