---
name: diagramming-uml
description: >-
  Authors UML and related text-based diagrams (PlantUML, Mermaid, D2, BPMN, and
  more). With the uml-mcp MCP server connected, uses generate_uml,
  validate_uml, list_diagram_types, and generate_uml_batch to return shareable
  Kroki URLs plus playground links when present. Activates for diagram requests,
  flowcharts, architecture or domain modeling, documentation visuals, readability
  or splitting overcrowded diagrams, or mentions of Kroki, uml-mcp, or diagram_type.
license: MIT
metadata:
  author: Antoine Boucher
---

> **Maintainers:** Keep the **`generate_uml`** table (especially `output_dir`) aligned with **uml-mcp** repo-local skills when you edit either side: [`.cursor/skills/uml-mcp-diagrams/SKILL.md`](https://github.com/antoinebou12/uml-mcp/blob/main/.cursor/skills/uml-mcp-diagrams/SKILL.md) and [`.skill/skills/uml-mcp-diagrams/SKILL.md`](https://github.com/antoinebou12/uml-mcp/blob/main/.skill/skills/uml-mcp-diagrams/SKILL.md). **Portable** prose (Smithery/agents everywhere) belongs in this **uml-skill** bundle; **repo-only** links (e.g. relative paths into **sequential-thinking** under `.skill`) stay in the uml-mcp copies only.

## Repo map

- **[README.md](README.md)** — install paths (Smithery, Cursor, clone) and high-level overview.
- **[AGENTS.md](AGENTS.md)** — read order for agents; links into this file and optional material.
- **[CLAUDE.md](CLAUDE.md)** — Claude Code entrypoint; same read order as **AGENTS.md**.
- **[references/DIAGRAM-TYPES.md](references/DIAGRAM-TYPES.md)** — extended intent → `diagram_type` (still confirm with **`uml://types`**).
- **[scripts/README.md](scripts/README.md)** — **`scripts/kroki_url.py`** when MCP is unavailable.

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

### Clarify purpose and notation

1. Clarify **purpose** and whether the user prefers **PlantUML**, **Mermaid**, **D2**, or “whatever fits best.”

### Resolve diagram type with server metadata

2. If **`diagram_type`** is unknown or ambiguous, read **`uml://types`** or **`uml://capabilities`** before authoring. If the client cannot read MCP resources, call **`list_diagram_types`** for the same metadata. Do **not** guess unsupported types or formats.

### Authoritative syntax with Context7

3. For **authoritative syntax** (Mermaid, PlantUML, D2, BPMN XML, etc.) when you are unsure of API or edge cases, use the **Context7** MCP if it is available in the client: resolve the relevant library ID, fetch focused docs, then author `code`. Do not substitute guesses for documented syntax.

### Outline structure

4. Outline main **participants**, **entities**, **flows**, or **layers** before writing full source.

### Plan large or ambiguous diagrams

5. For **ambiguous specs**, **large diagrams**, or **many lifelines or states**, plan before coding: apply ordered steps and revise when wrong (same principles as a sequential-thinking workflow). If a **sequential-thinking** MCP tool is available, prefer it for the planning phase (steps 1–4: type, scope, emphasis), then emit diagram source and call **`validate_uml`** / **`generate_uml`**.

### MCP diagram prompts

6. If the client exposes MCP **prompts**, prefer **`uml_diagram_with_thinking`** (plan then code) or **`uml_diagram`** before finalizing `code`. Type-specific prompts (e.g. `class_diagram`, `sequence_diagram`, `activity_diagram`, `c4_model`, `wireviz_harness`, `bpmn_executable_process`) are available when listed by the server.

### Write source from templates

7. Write source that matches the chosen **`diagram_type`**. Use **`uml://templates`**, **`uml://examples`**, and **`uml://mermaid-examples`** when you need starter syntax.

### Call validate_uml

8. Optionally call **`validate_uml`** with the same `diagram_type`, `code`, and planned `output_format`. Use **`strict: true`** for stricter Mermaid/D2 checks when needed.

### Call generate_uml

9. Call **`generate_uml`** (or **`generate_uml_batch`** for multiple independent diagrams in one round-trip). Handle **`error`** by fixing source or parameters, then present results (see below).

## When uml-mcp is available (preferred)

### Calling `generate_uml`

| Input | Guidance |
| --- | --- |
| `diagram_type` | Required. Must match a key from **`uml://types`** (e.g. `class`, `sequence`, `mermaid`, `d2`). |
| `code` | Required. Source in the language that matches `diagram_type`. |
| `output_dir` | Omit or `null` for **URL-first** output (no file write; responses may include **`content_base64`** when no directory is set). Set only if the user wants a saved image. |
| `output_format` | Default **`svg`** is usually best for URLs; use `png` / `pdf` / `jpeg` / `txt` only if valid for that type (see **`uml://formats`**). |
| `theme` | PlantUML-related types only; omit otherwise. |
| `scale` | SVG only; optional size multiplier. |

### Calling `validate_uml`

| Input | Guidance |
| --- | --- |
| `diagram_type`, `code`, `output_format` | Same intent as for **`generate_uml`**. |
| `strict` | Optional. Set **`true`** for stricter Mermaid/D2 structural checks before render. |

### Calling `generate_uml_batch`

Pass an **`items`** array of objects, each like **`generate_uml`** (`diagram_type`, `code`, optional `output_format`, `theme`, `scale`). Optional shared **`output_dir`** for all items. Per-index failures are returned without stopping the rest; use for multi-diagram documentation or comparisons.

### Intent to typical `diagram_type`

| User intent | Typical `diagram_type` |
| --- | --- |
| Classes, associations, packages | `class` (PlantUML) or `mermaid` with `classDiagram` |
| Messages over time, lifelines | `sequence` or Mermaid `sequenceDiagram` |
| Flows, swimlanes, process / BPMN | `activity`, `bpmn`, or Mermaid flowchart |
| Components, deployment | `component`, `deployment`, or C4-style types if listed in **`uml://types`** |
| Quick graphs, Gantt, pie | `mermaid` |
| Declarative layout / modern DSL | `d2` |
| ASCII box drawings, monospace diagrams | `ditaa` or `svgbob` (still use **`generate_uml`** with that `diagram_type`) |

When several types fit, pick the one the user named; otherwise prefer the type with the clearest template in **`uml://templates`**.

**Activity-style behavior:** For strict UML **activity** or **swimlanes**, **PlantUML `activity`** or **`bpmn`** is often clearer than Mermaid. If the user insists on **Mermaid**, use **`flowchart`** or **`stateDiagram-v2`** appropriately and set `diagram_type` to `mermaid` with valid Mermaid source.

### After `generate_uml`

1. On **`error`**, adjust `code`, `diagram_type`, or `output_format`, then retry (or re-run **`validate_uml`**).
2. Present clearly:
   - **`url`** — primary Kroki link to the rendered diagram.
   - **`playground`** — if present, link for interactive editing.
   - **`local_path`** — only when `output_dir` was set.
3. Keep a **short copy of `code`** in the reply so the user can edit and regenerate.

### Kroki (short context)

[Kroki](https://kroki.io/) is a unified HTTP API that renders many text diagram languages to SVG, PNG, PDF, etc. **uml-mcp** maps each supported **`diagram_type`** to a Kroki backend and may report a **`source`** field such as `kroki`, `plantuml_server`, or `mermaid_ink` depending on configuration and fallbacks.

**Logical vs Kroki path:** **`scripts/kroki_url.py`** takes a Kroki **URL path segment** (e.g. `plantuml`, `mermaid`, `d2`). uml-mcp **`generate_uml`** uses **logical** keys from **`uml://types`** (e.g. `class`, `sequence` → prepared PlantUML). Prefer MCP tools when connected; use the script only when MCP is unavailable.

### MCP tools and resources

- **Tools**: `generate_uml`, `validate_uml`, `list_diagram_types`, `generate_uml_batch`
- **Resources**: `uml://types`, `uml://formats`, `uml://templates`, `uml://examples`, `uml://capabilities`, `uml://server-info`, `uml://mermaid-examples`, `uml://bpmn-guide`, `uml://workflow`

**Prompts** (when the client exposes them): `uml_diagram`, `uml_diagram_with_thinking`, and type-specific prompts (`class_diagram`, `sequence_diagram`, `activity_diagram`, `usecase_diagram`, `mermaid_sequence_api`, `mermaid_gantt`, `bpmn_process_guide`, `c4_model`, `wireviz_harness`, `bpmn_executable_process`, `convert_class_to_mermaid`) help structure code before `generate_uml`.

Always use the **MCP APIs** exposed in the environment; do not guess unsupported `diagram_type` or `output_format` values when unsure — confirm with **`uml://types`** / **`uml://formats`**.

### Extended type and intent mapping

For **presentation intents** (timeline vs roadmap, Venn, quadrant, ER, architecture trees, optional constraints like `direction` / `max_nodes`) and a **summary table of Kroki `diagram_type` keys**, see **[references/DIAGRAM-TYPES.md](references/DIAGRAM-TYPES.md)**. Always still confirm keys and formats with **`uml://types`** and **`uml://formats`** for the connected server.

## When uml-mcp is not available

- Output diagram **source** in a fenced code block (`plantuml`, `mermaid`, `d2`, …).
- Mention that **uml-mcp** with Kroki would produce a live **`url`** for the same source when the server is connected.
- For proprietary GUI tools (Draw.io, Lucidchart, etc.), give **structured steps** or export hints, or offer **PlantUML/Mermaid/D2** as a portable alternative if the user asked for code.

## Authoring rules (always)

- Optimize for **[PURPOSE]** and **clarity**: minimal elements, consistent naming, readable layout.
- Tie prominent constructs to **[ELEMENT TYPE]** when it helps (e.g. testing → alternate flows; design → boundaries and interfaces).

## Readability (Kroki / DSL)

These rules apply to **diagram source** (Mermaid, PlantUML, D2, etc.), not to HTML layout:

- **Complexity**: Aim for roughly &lt;25 nodes in Mermaid and &lt;30 in PlantUML; fewer lifelines in sequence diagrams. If the user’s scope exceeds that, split into two diagrams or an overview plus a detail view.
- **Sequence**: Time flows **top → bottom**. Do not model upward message arrows. Use activation where the notation supports it (PlantUML `activate` / `deactivate`; Mermaid `activate` / `deactivate` when appropriate). Close every activation interval you open.
- **State and flowchart**: Label **every** transition or decision branch (event/guard or yes/no). Avoid drawing the same “to error” edge from every state; prefer one note, a group, or a single `*` / global exception path where the DSL allows.
- **Emphasis**: At most **one** strong visual highlight when the backend allows it (e.g. one Mermaid `classDef` / highlighted participant, one PlantUML `skinparam` / styled element, one D2 node style). Avoid rainbow or per-node rainbow fills — hierarchy and labels carry meaning.

## Prose around URLs

For **assistant-facing** explanations (summaries, caveats, next steps), you may use a **Humanizer** skill if it is installed in the client, to keep tone clear and direct. Do **not** run humanizer output through diagram `code` — Kroki needs valid DSL only.

## Related material in the uml-mcp repository

Contributors who work inside the **uml-mcp** repo may also use [`.cursor/skills/uml-mcp-diagrams/SKILL.md`](https://github.com/antoinebou12/uml-mcp/blob/main/.cursor/skills/uml-mcp-diagrams/SKILL.md) and [`.skill/skills/uml-mcp-diagrams/SKILL.md`](https://github.com/antoinebou12/uml-mcp/blob/main/.skill/skills/uml-mcp-diagrams/SKILL.md) as focused MCP workflow references. This **`SKILL.md`** stays self-contained for the **uml-skill** bundle.

## Extending this skill

Optional directories (add when needed):

- **`scripts/`** — small helpers agents can run. **`scripts/kroki_url.py`** prints a Kroki GET URL from diagram source (file or stdin); use when MCP is not available. See **`scripts/README.md`**.
- **`references/`** — extra docs on demand (e.g. **[references/DIAGRAM-TYPES.md](references/DIAGRAM-TYPES.md)**).
- **`assets/`** — templates, images, or data files.
