---
name: UML
description: >-
  Authors UML and related text-based diagrams (PlantUML, Mermaid, D2, BPMN, and
  more). When the uml-mcp MCP server is connected, use generate_uml and
  validate_uml to return shareable Kroki URLs plus playground links when
  present. Activate for diagram requests, flowcharts, architecture or domain
  modeling, documentation visuals, or mentions of Kroki, uml-mcp, or
  diagram_type.
license: MIT
metadata:
  author: Antoine Boucher
---

# UML (text diagrams + uml-mcp)

## Goal

Turn the user’s **[PROMPT]** into **correct, readable diagram source** for their **[PURPOSE]**, using **[TARGETLANGUAGE]** for labels. When **uml-mcp** is available, do not stop at raw source only: run **`generate_uml`** (and optionally **`validate_uml`**) so the user gets a **shareable Kroki `url`** and **`playground`** when present.

## When to activate

- User asks to create, draw, fix, or explain a **UML** or **diagram** (sequence, class, use case, activity, state, component, deployment, timing, and similar).
- User wants a **flowchart**, **BPMN**, **C4**, **ER**, **Gantt**, or other **text-rendered** diagram.
- User mentions **Kroki**, **uml-mcp**, **diagram_type**, or wants a **URL** to a rendered diagram instead of only code.

## Variables (use mentally or explicitly)

| Placeholder | Role |
| --- | --- |
| **DIAGRAM TYPE** | e.g. Sequence, Class, Activity, Component, State, Deployment, Use Case, BPMN, C4, Gantt, Mind map, … |
| **ELEMENT TYPE** | e.g. Actors, messages, objects, classes, interfaces, components, states, nodes, edges, swimlanes, constraints |
| **PURPOSE** | Communication, planning, design, analysis, modeling, documentation, implementation, testing, debugging |
| **DIAGRAMMING TOOL** | PlantUML, Mermaid, D2, or another type exposed by uml-mcp (`uml://types`) |
| **TARGETLANGUAGE** | Natural language for names and labels in the diagram |
| **PROMPT** | Concrete scenario, system, or question to depict |

Prefer **one** primary notation per diagram unless the user asks for a hybrid.

## Plan-then-generate workflow

1. Clarify **purpose** and whether the user prefers **PlantUML**, **Mermaid**, **D2**, or “whatever fits best.”
2. If **`diagram_type`** is unknown or ambiguous, read **`uml://types`** or **`uml://capabilities`** before authoring. Do **not** guess unsupported types or formats.
3. Outline main **participants**, **entities**, **flows**, or **layers** before writing full source.
4. If the client exposes MCP **prompts**, prefer **`uml_diagram_with_thinking`** (plan then code) or **`uml_diagram`** before finalizing `code`. Type-specific prompts (e.g. `class_diagram`, `sequence_diagram`, `activity_diagram`) are also available when listed by the server.
5. Write source that matches the chosen **`diagram_type`** backend. Use **`uml://templates`**, **`uml://examples`**, and **`uml://mermaid-examples`** when you need starter syntax.
6. Optionally call **`validate_uml`** with the same `diagram_type`, `code`, and planned `output_format`.
7. Call **`generate_uml`**, handle **`error`** by fixing source or parameters, then present results (see below).

Optional: if a separate **sequential-thinking** or similar MCP is available in the user’s environment, you may use it for long multi-diagram requests; this skill does **not** require it.

## When uml-mcp is available (preferred)

### Calling `generate_uml`

| Input | Guidance |
| --- | --- |
| `diagram_type` | Required. Must match a key from **`uml://types`** (e.g. `class`, `sequence`, `mermaid`, `d2`). |
| `code` | Required. Source in the language that matches `diagram_type`. |
| `output_dir` | Omit or `null` for **URL-only** (no file write). Set only if the user wants a saved image. |
| `output_format` | Default **`svg`** is usually best for URLs; use `png` / `pdf` / others only if valid for that type (see **`uml://formats`**). |
| `theme` | PlantUML-related types only; omit otherwise. |
| `scale` | SVG only; optional size multiplier. |

### Intent to typical `diagram_type`

| User intent | Typical `diagram_type` |
| --- | --- |
| Classes, associations, packages | `class` (PlantUML) or `mermaid` with class diagram syntax |
| Messages over time, lifelines | `sequence` or Mermaid sequence |
| Flows, swimlanes, process / BPMN | `activity`, `bpmn`, or Mermaid flowchart |
| Components, deployment | `component`, `deployment`, or C4-style types if listed in **`uml://types`** |
| Quick graphs, Gantt, pie | `mermaid` |
| Declarative layout / modern DSL | `d2` |
| ASCII-style box drawings | `ditaa` or `svgbob` (via **`generate_uml`** with that type) |

**Activity-style behavior:** For strict UML **activity** or **swimlanes**, **PlantUML `activity`** or **`bpmn`** is often clearer than Mermaid. If the user insists on **Mermaid**, use **`flowchart`** or **`stateDiagram-v2`** appropriately and set `diagram_type` to `mermaid` with valid Mermaid source.

### After `generate_uml`

1. On **`error`**, adjust `code`, `diagram_type`, or `output_format`, then retry (or re-run **`validate_uml`**).
2. Present **`url`**, **`playground`** if present, **`local_path`** only if `output_dir` was set, and a **short copy of `code`** for editing.

### Kroki (short context)

[Kroki](https://kroki.io/) is a unified HTTP API that renders many text diagram languages to SVG, PNG, PDF, etc. **uml-mcp** maps each supported **`diagram_type`** to a Kroki backend and may report a **`source`** field such as `kroki`, `plantuml_server`, or `mermaid_ink` depending on configuration and fallbacks.

### MCP tools and resources

- **Tools**: `generate_uml`, `validate_uml`
- **Resources**: `uml://types`, `uml://formats`, `uml://templates`, `uml://examples`, `uml://capabilities`, `uml://server-info`, `uml://mermaid-examples`, `uml://bpmn-guide`, `uml://workflow`

Always use the **MCP APIs** exposed in the environment; confirm types and formats with **`uml://types`** / **`uml://formats`** when unsure.

## When uml-mcp is not available

- Output diagram **source** in a fenced code block (`plantuml`, `mermaid`, `d2`, …).
- Mention that **uml-mcp** with Kroki would produce a live **`url`** for the same source when the server is connected.
- For proprietary GUI tools (Draw.io, Lucidchart, etc.), give **structured steps** or export hints, or offer **PlantUML/Mermaid/D2** as a portable alternative if the user asked for code.

## Authoring rules (always)

- Optimize for **[PURPOSE]** and **clarity**: minimal elements, consistent naming, readable layout.
- Tie prominent constructs to **[ELEMENT TYPE]** when it helps (e.g. testing → alternate flows; design → boundaries and interfaces).

## Related material in the uml-mcp repository

Contributors who work inside the **uml-mcp** repo may also use [.cursor/skills/uml-mcp-diagrams/SKILL.md](https://github.com/antoinebou12/uml-mcp/blob/main/.cursor/skills/uml-mcp-diagrams/SKILL.md) as a focused MCP workflow reference. This **`SKILL.md`** stays self-contained for the **uml-skill** bundle.

## Extending this skill

Optional directories (add when needed):

- **`scripts/`** — small helpers agents can run. **`scripts/kroki_url.py`** prints a Kroki GET URL from diagram source (file or stdin); use when MCP is not available. See **`scripts/README.md`**.
- **`references/`** — extra docs loaded on demand.
- **`assets/`** — templates, images, or data files.
