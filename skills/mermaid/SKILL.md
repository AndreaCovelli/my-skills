---
name: mermaid
description: Use this skill when the user wants Mermaid diagram code or asks to visualize a workflow, architecture, sequence, state machine, entity relationship, class model, timeline, gantt, or other structured system. Apply it when the task needs Mermaid-specific diagram selection, syntax, styling, or renderer-compatibility guidance.
---

# Mermaid

Create Mermaid diagrams that are structurally correct, easy to read in source form, and compatible with the renderer the user is targeting.

## Workflow

1. Identify the target renderer before writing syntax.
   Common cases: GitHub markdown, Mermaid Live Editor, VS Code preview, docs site, or another markdown renderer.
2. Choose the simplest diagram type that matches the user's intent.
3. Default to broadly compatible Mermaid syntax.
   Use newer or renderer-specific features only when the target environment is known to support them.
4. Keep the diagram focused.
   If it would exceed roughly 20 nodes or become visually dense, produce one overview diagram and note where a follow-up detail diagram would help.
5. If the user asks for polished styling, load [references/style-guide.md](references/style-guide.md).
6. If you need current syntax for a newer feature or a less common diagram type, load [references/syntax-links.md](references/syntax-links.md).

## Diagram Selection

Use these defaults unless the request clearly calls for something else:

| User intent | Preferred Mermaid type |
|---|---|
| Pipelines, workflows, architectures, decision flows | `flowchart` |
| Actor interactions over time | `sequenceDiagram` |
| Database schema | `erDiagram` |
| Object or domain models | `classDiagram` |
| State transitions | `stateDiagram-v2` |
| Schedules or delivery plans | `gantt` |
| Ordered milestones | `timeline` |

When the request is ambiguous, default to `flowchart LR`.

## Compatibility Defaults

- Prefer conservative syntax for shared markdown targets.
  Avoid renderer-sensitive features unless verified.
- For GitHub or other constrained renderers, prefer plain Mermaid without advanced config blocks or bleeding-edge syntax.
- Use semantic node IDs instead of single letters.
  `AUTH_SERVICE --> PRIMARY_DB` is better than `A --> B`.
- Keep labels short and consistent in grammatical form.
- Label non-obvious edges and relationships.

## Diagram-Specific Rules

### Flowcharts

- Use diamonds only for decisions.
- Use descriptive subgraph labels.
- Watch for Mermaid parser gotchas such as lowercase `end` in flowcharts.
  See [references/syntax-links.md](references/syntax-links.md) when needed.

### Sequence diagrams

- Add `autonumber` unless the user has a reason not to.
- Use `actor` for people or external systems and `participant` for internal services.
- Use `alt` and `else` for meaningful branches instead of flattening all paths.

### ER diagrams

- Mark keys when the schema is known.
- Use explicit relationship labels when they clarify the model.

### Class diagrams

- Add multiplicity where it matters.
- Label associations with a verb phrase when the relationship is not obvious.

### State diagrams

- Label transitions with the event or condition that triggers them.
- Include start and terminal states when the lifecycle matters.

## Styling

- Do not force styling when the user only wants valid Mermaid syntax.
- When style matters, prefer a small number of intentional theme decisions over heavy customization.
- Use `classDef` sparingly to encode meaning such as entry, exit, failure, or storage.
- For reusable palettes and config examples, load [references/style-guide.md](references/style-guide.md).

## Output

- Return Mermaid code in a fenced `mermaid` block unless the user asks for inline syntax only.
- If assumptions materially affect correctness, state them briefly after the diagram.
- If the renderer is unknown and compatibility is a risk, mention that the diagram uses conservative syntax and note any optional enhancements separately.
