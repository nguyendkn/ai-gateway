# Repository Guidelines

## Project Structure & Module Organization

This repository is a minimal scaffold for `ai-gateway`.

- `app/` is reserved for application source code. Keep runtime modules grouped by feature or service boundary.
- `docs/` is for architecture notes, API contracts, runbooks, and contributor-facing documentation.
- `.codex/` contains local agent configuration and should not be used for application code.

When adding tests, colocate narrow unit tests near the code they cover or use a top-level `tests/` directory for integration tests. Keep generated artifacts and build output out of source directories.

## Build, Test, and Development Commands

No package manifest, Makefile, or test runner is configured yet. Add project commands as tooling is introduced and keep this section updated.

Recommended command names:

- `make dev` or the package equivalent: run the gateway locally.
- `make test`: run the full automated test suite.
- `make lint`: run formatters, linters, and static checks.
- `make build`: produce deployable artifacts.

Prefer checked-in scripts over ad hoc shell commands so contributors use the same workflow.

## Coding Style & Naming Conventions

Follow the conventions of the language and framework selected for `app/`. Until tooling exists, use 2-space indentation for JavaScript/TypeScript/YAML/JSON and 4-space indentation for Python. Use descriptive file names and keep module names lowercase with separators where appropriate, for example `request-router.ts`, `provider_client.py`, or `config_loader.go`.

Keep functions small, name external integrations explicitly, and isolate provider-specific gateway logic from shared request, response, and policy code.

## Testing Guidelines

Tests are not configured yet. New behavior should include automated tests with clear scenario names, such as `routes_authenticated_requests` or `returns_provider_error_context`. Cover request validation, provider routing, error handling, retries, and configuration parsing before broad end-to-end tests.

Document required local services or environment variables in `docs/` and keep fixtures free of real secrets.

## Commit & Pull Request Guidelines

This repository has no commit history yet, so no project-specific commit convention is established. Use concise, imperative commit messages. Conventional Commit prefixes are recommended, for example `feat: add provider routing` or `test: cover retry policy`.

Pull requests should include a short summary, linked issue or task when available, test results, and notes for configuration or deployment changes. Include screenshots only for user-facing UI changes.

## Security & Configuration Tips

Do not commit API keys, provider tokens, customer data, or local `.env` files. Commit example configuration as `.env.example` or documented settings in `docs/`. Prefer explicit allowlists for providers, models, and outbound endpoints.
