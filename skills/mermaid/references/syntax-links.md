# Mermaid Syntax Links

Use this file when you need up-to-date Mermaid syntax, renderer compatibility details, or support for a newer diagram type.

## How To Use These Links

- Start with the central syntax reference.
- Open the specific diagram page only for the diagram type you need.
- Prefer conservative syntax if the renderer is unknown.
- Verify advanced syntax before using features such as icon shapes, newer diagram families, or config-heavy theming.

## Official Mermaid Docs

- Central syntax reference: https://mermaid.js.org/intro/syntax-reference
- Flowchart syntax: https://mermaid.js.org/syntax/flowchart
- Sequence diagrams: https://mermaid.js.org/syntax/sequenceDiagram.html
- Class diagrams: https://mermaid.js.org/syntax/classDiagram
- State diagrams: https://mermaid.js.org/syntax/stateDiagram.html
- Entity relationship diagrams: https://mermaid.js.org/syntax/entityRelationshipDiagram.html
- Gantt diagrams: https://mermaid.js.org/syntax/gantt.html
- Timeline diagrams: https://mermaid.js.org/syntax/timeline.html
- Configuration: https://mermaid.js.org/config/setup/mermaid/interfaces/MermaidConfig.html
- Theming: https://mermaid.js.org/config/theming.html
- ELK config schema: https://mermaid.js.org/config/schema-docs/config-properties-elk.html

## Current High-Value Gotchas

- In flowcharts, lowercase `end` can break parsing. Use `End`, `END`, or another label form.
- In flowcharts, connectors involving leading `o` or `x` can be parsed as special edge syntax.
- Advanced syntax and config behavior can vary across GitHub, IDE previews, docs sites, and the Mermaid Live Editor.

Checked against official Mermaid docs on 2026-04-27.
