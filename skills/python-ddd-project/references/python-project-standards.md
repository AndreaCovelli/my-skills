# Python Project Standards Reference

Load this file only when the task needs detailed conventions for project setup,
review, migration, or repo-wide consistency.

## Repository Layout

Preferred layout:

```text
/
  /src
    /{namespace}
      /domain
      /infrastructure
      /interfaces
  /tests
    /unit
    /integration
  pyproject.toml
  uv.lock
```

Guidance:

- Prefer a `src/` layout for packages and applications.
- Keep the domain layer free of ORM, HTTP, and other infrastructure dependencies.
- Keep infrastructure adapters from leaking persistence models upward.
- Keep interface code thin: web handlers, CLI entrypoints, jobs, and TUI glue belong here.

## Tooling

Default stack for repos using this skill:

- Package and environment management: `uv`
- Linting and formatting: `ruff`
- Tests: `pytest`
- Settings and environment parsing: `pydantic-settings`
- Dependency updates: `Dependabot`

Apply these as conventions for matching repos, not as universal Python rules for every project.

## Commands

Run commands from the repository root.

| Task | Preferred command |
|---|---|
| Sync dependencies | `uv sync` |
| Run tests | `uv run pytest` |
| Lint | `uvx ruff check` |
| Lint with autofix | `uvx ruff check --fix` |
| Format | `uvx ruff format` |
| Type check | `uv run pyright` or `uv run mypy --strict` |
| Run a single-file script | `uv run script.py` |

Notes:

- Prefer project-declared dependencies over ad-hoc installs.
- If a repo already uses another package workflow intentionally, do not switch it unless asked.
- PEP 723 inline metadata is appropriate for standalone scripts when the repo uses `uv`.

## Configuration

Preferred pattern:

- Use `BaseSettings` from `pydantic-settings` for application settings.
- Configure settings via `SettingsConfigDict(...)`.
- Use `Field(alias=...)` or `Field(validation_alias=...)` when environment variable names must differ from Python field names.

Guidance:

- Prefer validated settings objects over scattered `os.environ` access.
- If direct environment reads already exist in a legacy codepath, do not rewrite them unless the task touches that area or the user asks for cleanup.

## Python Rules

- Add type hints at module and function boundaries.
- Avoid implicit `Any` in new code when practical.
- Prefer explicit result values for expected domain failures when that improves clarity.
- Do not use exceptions as normal business-flow control when a returned domain outcome is clearer.
- Use modern Python features when they materially simplify the code and match the repo's target version.

## Testing

- Use `pytest` for new tests in repos following this stack.
- Structure tests clearly with Arrange, Act, Assert.
- Use fixtures for dependency setup and test composition.
- Use `@pytest.mark.parametrize` for data-driven cases.
- Use property-based tests selectively for invariants and edge-heavy logic.

## CI/CD and Automation

### Commits and PRs

- Prefer Conventional Commits when the repo already uses them.
- PR descriptions should summarize behavior changes, structural changes, and validation performed.

### Docker

- Prefer multi-stage builds for production images.
- Copy dependency metadata such as `pyproject.toml` and `uv.lock` early to improve layer caching.
- Keep the runtime image minimal and copy only what is needed.

### GitHub Actions

- Prefer workflow triggers scoped with `paths` or `paths-ignore` when the repo has enough automation for this to matter.
- Treat caches as untrusted data. Avoid writing caches from untrusted pull request contexts.
- Use trusted publishing with OIDC for PyPI releases when the project publishes packages.

### Dependabot

- Configure updates in `.github/dependabot.yml` on the default branch.
- For `uv` projects, prefer `package-ecosystem: "uv"`.
- Add `github-actions` updates when workflows pin external actions.
- Prefer grouped weekly updates unless the repo has a reason to use another cadence.
- If the repo uses `exclude-newer` in `uv`, consider a matching Dependabot `cooldown`.

## Review Heuristics

When reviewing a repo or change set against these standards, check:

- whether the repo actually uses this stack
- whether code crosses domain, infrastructure, and interface boundaries
- whether dependency changes fit the current package workflow
- whether tests and automation match the repo's established conventions
- whether a proposed cleanup is worth the migration cost
