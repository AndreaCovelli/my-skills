# my-skills

Installable Codex skills repository for reusable workflows and project conventions.

This repository follows the `skills/` layout used by the Vercel skills ecosystem, so each
skill can be installed independently and kept versioned alongside its prompt instructions.

## Included skills

| Skill | Purpose |
|---|---|
| `mermaid` | Create, edit, optimize, and style Mermaid.js diagrams for architecture, workflows, ERDs, and other diagram-as-code use cases. |
| `python-ddd-project` | Enforce Python project standards built around `uv`, `ruff`, `pydantic` v2, `pytest`, Dependabot, DDD layering, and CI/CD conventions. |

## Repository layout

```text
skills/
├── mermaid/
│   └── SKILL.md
└── python-ddd-project/
    └── SKILL.md
```

Each skill folder contains its own `SKILL.md` with:

- YAML frontmatter used for discovery and triggering
- Task-specific operating instructions for the agent
- Conventions, commands, and constraints relevant to that skill

## Install

List the installable skills in this repository:

```bash
npx skills add AndreaCovelli/my-skills --list
```

Install a single skill locally:

```bash
npx skills add AndreaCovelli/my-skills --skill mermaid
npx skills add AndreaCovelli/my-skills --skill python-ddd-project
```

Install a skill globally for a specific agent:

```bash
npx skills add AndreaCovelli/my-skills --skill mermaid -g -a claude-code
npx skills add AndreaCovelli/my-skills --skill python-ddd-project -g -a claude-code
```

## When to use these skills

- Use `mermaid` when working on diagrams as code, especially flowcharts, sequence diagrams, state diagrams, ER diagrams, Gantt charts, and architecture visuals.
- Use `python-ddd-project` when the task involves a Python codebase that should follow modern project standards and a strong domain-driven structure.

## Notes

- Skills are versioned through this repository, so changes to prompts and conventions can be reviewed like normal source code.
- Adding a new skill typically means creating a new `skills/<skill-name>/SKILL.md` directory and documenting it here.

## Reference

- Vercel skills project: https://github.com/vercel-labs/skills
