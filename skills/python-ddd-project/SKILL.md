---
name: python-ddd-project
description: >
  Enforces modern Python project standards for codebases using uv, ruff, pydantic V2,
  structlog, logfire, and pytest with Domain-Driven Design (DDD) and Twelve-Factor App
  principles. Use this skill whenever working in a Python project — writing new code,
  adding dependencies, setting up CI/CD, writing tests, structuring modules, or reviewing
  architecture. Also trigger when the user mentions uv, ruff, pydantic, structlog, logfire,
  SQLAlchemy, pyproject.toml, DDD layers, src/ layout, or any of the toolchain items below.
  When in doubt, load this skill — it is the authoritative source for all project conventions.
---

# Python DDD Project Standards

A skill for enforcing consistent, enterprise-grade Python project conventions across the
full stack: tooling, architecture, language rules, testing, and CI/CD.

---

## Directory Layout

Strict `src/` layout. Flat layout is **not permitted**.

```
/
  /src
    /{namespace}
      /domain          # Core business logic — pure Python, zero infrastructure deps
      /infrastructure  # Persistence (SQLAlchemy), external API clients
      /interfaces      # Web endpoints, CLI, TUI
  /tests
    /unit
    /integration
  /docs                # MkDocs Material documentation
  pyproject.toml       # Single source of truth — no setup.py or setup.cfg
  uv.lock              # Deterministic lockfile, always committed
```

---

## Setup & Commands

Run all commands from the repository root:

| Task | Command |
|---|---|
| Bootstrap / sync deps | `uv sync` |
| Run tests | `uv run pytest` |
| Lint + autofix | `uvx ruff check --fix` |
| Format | `uvx ruff format` |
| Type check | `uv run pyright` or `uv run mypy --strict` |
| Single-file scripts | `uv run script.py` (use PEP 723 inline metadata) |

**Never** use `pip install`, `python -m venv`, `poetry`, `pdm`, or `pipenv`.

---

## Architecture Rules

### Domain-Driven Design (DDD)
- **Domain layer**: Pure Python only. Zero infrastructure imports. No ORMs, no HTTP clients.
- **Infrastructure layer**: All persistence (SQLAlchemy) and external API calls live here. Always return pure Python domain objects — never leak ORM models upward.
- **Interfaces layer**: Web endpoints, CLI entrypoints, TUI. Depends on domain, never on infrastructure directly.

### Configuration
- Use `pydantic-settings` (`BaseSettings`) **exclusively** for all config and environment variables.
- **Never** use `os.environ` directly or manual string parsing.

---

## Ecosystem & Toolchain

| Concern | Tool | Forbidden alternatives |
|---|---|---|
| Package management | `uv` | pip, poetry, pdm, pipenv |
| Linting + formatting | `ruff` | black, isort, flake8 |
| Data validation | `pydantic` V2 | marshmallow, attrs (for validation) |
| Logging | `structlog` (structured JSON) | `logging` with string interpolation, `print()` |
| Observability | `logfire` (OpenTelemetry) | manual metrics, bare OTEL SDK |
| Documentation | MkDocs Material + `mkdocstrings` | Sphinx |

Docstrings must follow **Google or NumPy format**.

---

## Python 3.14 Language Rules

- **Type hints everywhere.** All function signatures, variables, and module boundaries must be strictly typed. No implicit `Any`.
- **Result pattern for domain failures.** Return success/error variants for expected failures (e.g. invalid transaction). Do not use exceptions for control flow.
- **Exceptions for catastrophes only.** Reserve `raise` for true infrastructural failures (DB down, network timeout, etc.).
- **`ExceptionGroup` + `except*`** for handling multiple unrelated concurrent failures in `asyncio`.
- **Structural pattern matching** (`match`/`case`) to introspect exception payloads and route error logic.

---

## Testing

- **Framework:** `pytest` exclusively. Never subclass `unittest.TestCase`.
- **Structure:** Every test function must follow **Arrange -> Act -> Assert (AAA)**.
- **State:** Use `pytest` fixtures for dependency injection and mocking. No `setUp`/`tearDown`.
- **Parametrize:** Use `@pytest.mark.parametrize` for data-driven cases.
- **Property-based:** Use `hypothesis` for boundary and invariant testing.

---

## CI/CD, Docker & GitHub Actions

### Commits & PRs
- **Conventional Commits**: `feat: ...`, `fix: ...`, `chore: ...`, etc.
- PR descriptions must detail the behavior change and structural impacts.

### Docker (Multi-stage builds — mandatory)
```
Stage 1 (Builder):  OS base image -> compile C-extensions via `uv sync`
Stage 2 (Production): Minimal slim/distroless image -> copy only compiled artifacts + source
```
Copy `pyproject.toml` and `uv.lock` **first** to maximize layer caching.

### GitHub Actions
- Use `paths` / `paths-ignore` so workflows only trigger on relevant file changes.
- GitHub Actions cache writes (e.g. `setup-uv`) are **write-only on trusted `push` to `main`**. PRs use restore-only mode to prevent cache poisoning.
- Use **Trusted Publishers (OIDC)** for PyPI releases. Never hardcode API tokens.

---

## Maintenance Note

When tooling, architecture patterns, or CI/CD standards in a project evolve, update the
relevant `AGENTS.md` or skill file to keep conventions accurate for future sessions.
