---
name: python-ddd-project
description: >
  Apply project standards for Python repositories that use uv, ruff, pytest,
  pydantic-settings, and a Domain-Driven Design style src/ layout. Use when
  adding code, dependencies, tests, CI, configuration, or project structure in
  repos that follow this stack, or when the user explicitly asks for these
  conventions.
---

# Python DDD Project

Use this skill only for Python repositories that already use, or should be
standardized on, the `uv` + `ruff` + `pytest` + `pydantic-settings` stack with
DDD-style layering.

## Required Inputs

- The repository contents, especially `pyproject.toml`, source layout, tests, and CI files.
- Any existing project conventions in `AGENTS.md`, docs, or prior user instructions.
- The user request: feature work, bug fix, dependency change, repo setup, review, or refactor.

## Workflow

1. Inspect the repository before changing anything.
   Check `pyproject.toml`, the package layout under `src/`, test structure, and automation files.
2. Decide whether this skill applies fully, partially, or not at all.
   If the repo already follows different explicit conventions, preserve them unless the user asked to migrate.
3. Apply the core standards during the task.
   Use `uv` for project and dependency commands, `ruff` for linting and formatting, `pytest` for tests, and typed Python interfaces at module boundaries.
4. Keep architecture boundaries clear.
   Put domain logic in the domain layer, infrastructure concerns in infrastructure code, and delivery concerns in interface code.
5. Make the smallest coherent change that satisfies the request.
   Avoid broad rewrites unless the task is specifically about standardization or migration.
6. Validate with the repo's available checks.
   Prefer the project's existing test, lint, and type-check commands. If they are missing, use the conventions in [references/python-project-standards.md](references/python-project-standards.md).

## Final Checks

- The change matches the repo's actual stack and does not impose unrelated tooling.
- New code, tests, and configuration follow the same conventions consistently.
- Dependency and CI changes are aligned with `uv`-based workflows when this repo uses `uv`.
- If you had to infer conventions, state that clearly in the final response.

## Reference Guide

Read [references/python-project-standards.md](references/python-project-standards.md) when you need the detailed standards for:

- repository layout
- dependency management
- configuration patterns
- testing rules
- CI/CD, Docker, and Dependabot guidance

Read [references/sources.md](references/sources.md) when you need the underlying documentation links for those conventions.
