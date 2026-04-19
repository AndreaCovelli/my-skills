---
name: mermaid
description: "Use this skill whenever the user wants to create, edit, optimize, or style Mermaid.js diagrams. Triggers include: any mention of 'flowchart', 'Mermaid', 'diagram as code', 'sequence diagram', 'state diagram', 'ER diagram', 'class diagram', 'Gantt chart', 'Kanban', 'architecture diagram', or requests to visualize systems, pipelines, workflows, data flows, or software architecture. Also use when the user asks to theme, style, or improve the aesthetics of an existing diagram, integrate icons, configure layout engines, or generate diagrams from descriptions. Do NOT use for GUI-based tools like Visio, Lucidchart, or Miro, or for rendering/exporting diagrams to image formats without Mermaid syntax."
---

# Mermaid.js Diagramming

## Overview

Mermaid.js is a JavaScript-based rendering engine that converts Markdown-inspired text definitions into dynamic SVG diagrams. It supports version control, diff reviews, and co-location with source code, eliminating "doc-rot" from disconnected GUI-based documentation tools.

## Quick Reference

| Goal | Approach |
|------|----------|
| Simple flowchart | `flowchart TD` + bracket/paren node syntax |
| Complex topology | `flowchart LR` + ELK layout engine + semantic node IDs |
| Brand-aligned styling | `base` theme + `themeVariables` hex overrides |
| Icon-rich infra diagram | Iconify framework + `@iconify-json/logos` or `@iconify-json/aws` |
| Avoid platform stripping | Pre-render to SVG/PNG via `mmdc` CLI in CI/CD |
| Hand-drawn look | `look: handDrawn` + optional `handDrawnSeed` |
| Grouped services | `subgraph` blocks + invisible padding workaround |

---

## Diagram Types

| Type | Best For |
|------|----------|
| `flowchart` | Pipelines, decision trees, system architecture |
| `sequenceDiagram` | API call chains, auth handshakes, async events |
| `stateDiagram-v2` | Workflows, object lifecycles, FSMs |
| `erDiagram` | Database schemas, cardinalities |
| `classDiagram` | OOP documentation, interface notation |
| `gantt` | Project timelines, task dependencies |
| `pie` | Data proportions (up to 12 slices) |
| `kanban` | Agile boards (Todo / In Progress / Done) |
| `architecture-beta` *(v11.1+ syntax)* | Cloud topology, VPC/service grouping |

---

## Flowchart Orientation

```
flowchart LR   ← best for pipelines, user journeys (reads left-to-right)
flowchart TD   ← best for hierarchies, decision trees, layered architectures
flowchart BT   ← bottom-up trees
flowchart RL   ← right-to-left flow
```

**Rule:** use `LR` for sequential data flows; use `TD` for hierarchical or layered structures.

---

## Node Syntax

### Classic shorthand (fast drafting)
```
id[Rectangle]
id(Rounded)
id((Circle))
id{Diamond}
id[(Database)]
```

### Modern semantic syntax (v11.4+, preferred for complex diagrams)
```
id@{ shape: rect, label: "My Node" }
id@{ shape: cylinder, label: "Postgres DB" }
id@{ shape: diamond, label: "Decision?" }
id@{ shape: stadium, label: "Start / End" }
```

### Naming conventions
```
❌ Bad:  A --> B --> C
✅ Good: WEB_PORTAL --> AUTH_SERVICE --> DB_CACHE
```
Use semantic IDs. Alphabet soup is unreadable at scale.

### Text formatting inside nodes
```
NODE["**Bold** and _italic_ label"]
NODE["`Markdown rendered label`"]
NODE["Line 1<br>Line 2"]   ← manual line breaks
```

> **Warning:** Never use the standalone word `end`, or start an edge with isolated `o` or `x` — these break the parser. Wrap them in quotes or capitalize.

---

## Layout Engines

### Choosing an engine

| Engine | Algorithm | Use Case | Notes |
|--------|-----------|----------|-------|
| `dagre` *(default)* | Directed Acyclic Graph | Simple flows, basic trees | Fast; struggles with dense edge crossings |
| `elk` | Eclipse Layout Kernel | Complex topologies, nested subgraphs | Best crossing minimization |
| `tidy-tree` | Hierarchical Tree | Org charts, binary trees | Zero crossings for pure trees; fails on cycles |
| `cose-bilkent` | Force-Directed | Network maps, cyclical graphs | Ignores hierarchy; spaces nodes via repulsion |

### Activating ELK (recommended for complex diagrams)

Via YAML frontmatter (local, preferred):
```yaml
---
config:
  layout: elk
---
```

Via JS initialization (global):
```js
mermaid.initialize({ layout: 'elk' })
```

### ELK tuning parameters (documented config keys)

```yaml
---
config:
  layout: elk
  elk:
    mergeEdges: false
    nodePlacementStrategy: BRANDES_KOEPF
    cycleBreakingStrategy: GREEDY_MODEL_ORDER
    forceNodeModelOrder: false
    considerModelOrder: NODES_AND_EDGES
---
```

Use only the ELK keys Mermaid documents in its config schema.

---

## Theming and Aesthetics

### Built-in themes

| Theme | Use Case |
|-------|----------|
| `default` | Generic web display |
| `neutral` | Black-and-white print |
| `dark` | Low-light interfaces |
| `forest` | Earthy, nature-inspired |
| `base` | **Blank canvas for full customization** ← use this |

### Full custom theming (base theme)

```yaml
---
config:
  theme: base
  themeVariables:
    background: "#1e1e2e"
    mainBkg: "#313244"
    primaryColor: "#313244"
    primaryBorderColor: "#cba6f7"
    primaryTextColor: "#cdd6f4"
    lineColor: "#f38ba8"
    clusterBkg: "#181825"
    clusterBorder: "#6c7086"
    tertiaryColor: "#fab387"
---
```

> **Critical:** Only hex codes work (e.g. `#ff0000`). Named CSS colors like `"red"` will fail silently.

### Catppuccin Mocha palette

| Variable | Hex | Role |
|----------|-----|------|
| `background` / `mainBkg` | `#181926` | Canvas backdrop |
| `primaryColor` | `#303446` | Node fill |
| `primaryBorderColor` | `#8839ef` | Node stroke |
| `primaryTextColor` | `#cdd6f4` | Node text |
| `lineColor` | `#f38ba8` | Edges & arrowheads |
| `clusterBkg` | `#414559` | Subgraph fill |
| `clusterBorder` | `#6c7086` | Subgraph border |
| `tertiaryColor` | `#fab387` | Alternate nodes |

### Tokyo Night palette

| Variable | Hex | Role |
|----------|-----|------|
| `background` / `mainBkg` | `#1a1b26` | Canvas backdrop |
| `primaryColor` | `#24283b` | Node fill |
| `primaryBorderColor` | `#7aa2f7` | Node stroke |
| `primaryTextColor` | `#c0caf5` | Node text |
| `lineColor` | `#ff9e64` | Edges & arrowheads |
| `clusterBkg` | `#292e42` | Subgraph fill |
| `clusterBorder` | `#565f89` | Subgraph border |
| `tertiaryColor` | `#e0af68` | Alternate nodes |

### Hand-drawn look

```yaml
---
config:
  look: handDrawn
  handDrawnSeed: 42    # integer ensures deterministic rendering for CI/CD
---
```
Best paired with ELK. Ideal for conceptual docs and agile ideation.

---

## Semantic Styling with classDef

```
flowchart LR
  classDef critical fill:#f38ba8,stroke:#d20f39,color:#1e1e2e,stroke-width:2px
  classDef db fill:#fab387,stroke:#fe640b,color:#1e1e2e
  classDef invisible stroke:none,fill:none,display:none

  AUTH_SERVICE:::critical
  PRIMARY_DB:::db
```

### Per-edge styling

```
linkStyle 0 stroke:#f38ba8,stroke-width:2px
linkStyle 1 stroke:#a6e3a1,stroke-width:1px,stroke-dasharray:5
```

Uses zero-based index of edge definitions.

> **Edge arrowhead fix:** Changing `linkStyle` stroke color does NOT change the arrowhead fill. Target `#L-nodeA-nodeB .arrowheadPath` with a CSS `!important` override in a `<style>` block.

---

## Subgraphs and Structural Hierarchy

### Basic subgraph

```
flowchart LR
  subgraph VPC["Production VPC"]
    direction TB
    API_GW --> LAMBDA
    LAMBDA --> RDS
  end
```

### Invisible padding workaround

The Mermaid engine clips subgraph titles into border lines on nested graphs. Fix with ghost nodes:

```
flowchart TD
  subgraph OUTER["My Service"]
    subgraph padding[ ]
      subgraph INNER["Inner Logic"]
        NODE_A --> NODE_B
      end
    end
  end

  classDef invisible stroke:none,fill:none,display:none
  class padding invisible
```

This forces the layout engine to reserve spatial padding without rendering anything visible.

---

## Iconify Integration (v11.4+)

Replaces brittle FontAwesome `fa:` syntax with full-color SVG icon packs.

### Register packs (official Mermaid API examples)

```js
import mermaid from 'mermaid';

mermaid.registerIconPacks([
  {
    name: 'logos',
    loader: () => import('@iconify-json/logos').then((module) => module.icons),
  },
]);
```

### Use in node definitions

```
DB_NODE@{ shape: cylinder, icon: "logos:postgresql", label: "PostgreSQL" }
JS_NODE@{ icon: "logos:javascript", label: "JavaScript" }
```

### Available packs

| Pack | Contents |
|------|----------|
| `@iconify-json/logos` | Programming languages, frameworks, services |
| `@iconify-json/aws` | AWS service icons |
| `@iconify-json/azure` | Azure service icons |

For CDN-based usage, Mermaid documents `loader` functions that fetch the Iconify JSON directly. Prefer the documented `registerIconPacks()` API over ad hoc CLI flags.

---

## Interactivity

```
flowchart LR
  API_GW --> AUTH_SERVICE

  click API_GW "https://docs.example.com/api" "Open API docs"
  click AUTH_SERVICE call openAuthPanel()
```

> **Security:** Interactivity and CSS injections require `securityLevel: 'loose'`. This is disabled by default to prevent XSS. Only enable in trusted, controlled environments.

```js
mermaid.initialize({ securityLevel: 'loose' })
```

---

## Platform Portability

| Platform | Frontmatter | Custom CSS | ELK | Click Events |
|----------|-------------|------------|-----|--------------|
| VSCode (MPE) | ✅ | ✅ | ✅ | ✅ |
| Obsidian | ✅ | ✅ | ✅ | ✅ |
| Notion | ⚠️ Partial | ❌ | ❌ | ❌ |
| GitHub README | ❌ Stripped | ❌ | ❌ | ❌ |

**GitHub specific issues:**
- Strips all `%%{init:...}%%` and YAML frontmatter config
- Reverts to Dagre + default theme
- Dark mode often renders dark text on dark nodes (illegible)

### The gold-standard workaround: pre-render in CI/CD

```yaml
# .github/workflows/diagrams.yml
- name: Render diagrams
  run: |
    npm install -g @mermaid-js/mermaid-cli
    mmdc -i docs/architecture.md -o docs/architecture.svg \
         -t base -b transparent
- name: Commit SVG
  run: git add docs/*.svg && git commit -m "chore: update rendered diagrams"
```

Author in Mermaid text → compile to SVG/PNG in CI → embed static image in Markdown. Preserves all diffs, version control, and review benefits while guaranteeing pixel-perfect output on any platform.

---

## Advanced CSS Injections

For drop shadows and gradients not achievable via `classDef`:

```html
<style>
/* Drop shadow on critical nodes */
.node.critical rect {
  filter: drop-shadow(0px 4px 6px rgba(243, 139, 168, 0.4));
}

/* Fix arrowhead color mismatch */
#L-AUTH_SERVICE-DB .arrowheadPath {
  fill: #f38ba8 !important;
}
</style>
```

For gradient fills, declare a `<linearGradient>` in SVG namespace and reference its ID via CSS `fill`.

> Requires `securityLevel: 'loose'` to take effect in browser environments.

---

## Typography Best Practices

- For multi-line node labels, add `text-align: left` via `classDef` to avoid ragged centered text
- Use `markdownAutoWrap: false` in frontmatter when precise line breaks via `<br>` are needed
- Color is **semantic**, not decorative — red = failure state, green = healthy, yellow = warning
- Icons replace ambiguous shapes: a cylinder icon for Postgres beats a grey rectangle every time

---

## Complexity Management Rules

1. **80/20 rule:** A diagram showing 80% of the logic with perfect clarity beats 100% coverage with zero readability.
2. **Modularize:** One high-level architecture diagram + separate zoomed-in sub-diagrams per service.
3. **Comment the source:** Use `%% Section: Auth Flow %%` between subgraphs in the raw Markdown.
4. **ELK for anything non-trivial:** If you have more than ~10 nodes or any cross-layer edges, switch to ELK.
5. **Semantic IDs always:** `AUTH_SERVICE` not `A`. Future you will thank present you.

---

## References

- Configuration: https://mermaid.js.org/config/configuration
- Layouts: https://mermaid.js.org/config/layouts
- ELK config schema: https://mermaid.js.org/config/schema-docs/config-properties-elk.html
- Theme configuration: https://mermaid.js.org/config/theming.html
- Registering icon packs: https://mermaid.js.org/config/icons.html
- Architecture diagram syntax: https://mermaid.js.org/syntax/architecture.html
