# Mermaid Style Guide

Use this file only when the user asks for a polished, presentation-ready diagram or when visual refinement is part of the task.

## Styling Principles

- Match style to context instead of decorating by default.
- Keep one visual hierarchy per diagram.
- Use shape, color, and edge labels to clarify meaning, not to add noise.
- Prefer a few semantic `classDef` styles over many one-off node styles.

## Safe Defaults

- Use `flowchart LR` for most process and architecture diagrams.
- If the graph is dense or has many crossing edges, consider `layout: elk` when the renderer supports it.
- Keep node labels short enough to scan without wrapping excessively.

## Shape Conventions For Flowcharts

| Meaning | Suggested syntax |
|---|---|
| Entry or exit point | `([Label])` |
| Decision | `{Label}` |
| Process step | `[Label]` |
| Event | `((Label))` |
| Input or output | `[/Label/]` |
| Reusable routine | `[[Label]]` |
| Storage or database | `[(Label)]` |

Use the same shape consistently for the same meaning within one diagram.

## Example Config Block

Use only when the target renderer supports Mermaid config blocks:

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#1e293b"
    primaryTextColor: "#f8fafc"
    primaryBorderColor: "#475569"
    lineColor: "#64748b"
    clusterBkg: "#f1f5f9"
    clusterBorder: "#cbd5e1"
    titleColor: "#0f172a"
    edgeLabelBackground: "#ffffff"
---
flowchart LR
    START([Start]) --> STEP[Process]
    STEP --> END([End])
```

## Palette Defaults

### Slate

Use for technical architecture and infrastructure diagrams.

```yaml
primaryColor: "#1e293b"
primaryTextColor: "#f8fafc"
primaryBorderColor: "#475569"
lineColor: "#64748b"
clusterBkg: "#f1f5f9"
clusterBorder: "#cbd5e1"
titleColor: "#0f172a"
edgeLabelBackground: "#ffffff"
```

### Indigo

Use for product flows, SaaS systems, and user journeys.

```yaml
primaryColor: "#6366f1"
primaryTextColor: "#ffffff"
primaryBorderColor: "#4338ca"
lineColor: "#818cf8"
clusterBkg: "#eef2ff"
clusterBorder: "#c7d2fe"
titleColor: "#312e81"
edgeLabelBackground: "#f5f3ff"
```

### Emerald

Use for pipelines, CI/CD, and data movement.

```yaml
primaryColor: "#059669"
primaryTextColor: "#ffffff"
primaryBorderColor: "#047857"
lineColor: "#34d399"
clusterBkg: "#f0fdf4"
clusterBorder: "#a7f3d0"
titleColor: "#064e3b"
edgeLabelBackground: "#f0fdf4"
```

### Amber

Use for business processes and general documentation.

```yaml
primaryColor: "#d97706"
primaryTextColor: "#ffffff"
primaryBorderColor: "#b45309"
lineColor: "#fbbf24"
clusterBkg: "#fffbeb"
clusterBorder: "#fde68a"
titleColor: "#78350f"
edgeLabelBackground: "#fffbeb"
```

### Rose

Use for error handling, security, and incident flows.

```yaml
primaryColor: "#e11d48"
primaryTextColor: "#ffffff"
primaryBorderColor: "#be123c"
lineColor: "#fb7185"
clusterBkg: "#fff1f2"
clusterBorder: "#fecdd3"
titleColor: "#881337"
edgeLabelBackground: "#fff1f2"
```

## Semantic `classDef` Example

```mermaid
classDef entry fill:#6366f1,stroke:#4338ca,color:#fff,font-weight:bold
classDef exit fill:#059669,stroke:#047857,color:#fff,font-weight:bold
classDef error fill:#e11d48,stroke:#be123c,color:#fff,font-weight:bold
classDef storage fill:#1e293b,stroke:#475569,color:#f8fafc
```

Use semantic classes only where they improve readability.
