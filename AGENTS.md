# uml-skill — agent notes

This repository is the **diagramming-uml** portable skill bundle (`name` in [SKILL.md](SKILL.md) frontmatter). It is not an application monorepo: the canonical workflow and MCP tables live in **one** file.

## Read order

1. **[SKILL.md](SKILL.md)** — full skill: activation, [plan-then-generate workflow](SKILL.md#plan-then-generate-workflow), `generate_uml` / `validate_uml` / `list_diagram_types`, Kroki context, authoring rules.
2. **[references/DIAGRAM-TYPES.md](references/DIAGRAM-TYPES.md)** — when you need extended intent → `diagram_type` tables; always still confirm keys with **`uml://types`** on the connected server.
3. **[scripts/README.md](scripts/README.md)** — when **uml-mcp** is not available and you only need a Kroki GET URL via **`kroki_url.py`**.

Install and Smithery paths: **[README.md](README.md)**.

## Workflow anchors (same document)

Deep links into [SKILL.md](SKILL.md) for the ordered steps:

- [Clarify purpose and notation](SKILL.md#clarify-purpose-and-notation)
- [Resolve diagram type with server metadata](SKILL.md#resolve-diagram-type-with-server-metadata)
- [Authoritative syntax with Context7](SKILL.md#authoritative-syntax-with-context7)
- [Outline structure](SKILL.md#outline-structure)
- [Plan large or ambiguous diagrams](SKILL.md#plan-large-or-ambiguous-diagrams)
- [MCP diagram prompts](SKILL.md#mcp-diagram-prompts)
- [Write source from templates](SKILL.md#write-source-from-templates)
- [Call validate_uml](SKILL.md#call-validate_uml)
- [Call generate_uml](SKILL.md#call-generate_uml)

## Related projects

- **[uml-mcp](https://github.com/antoinebou12/uml-mcp)** — MCP server and `uml://*` resources.
- **Smithery:** [diagramming-uml](https://smithery.ai/skills/antoinebou12/uml) (`antoinebou12/uml`).
- **Deployed MCP (example):** [https://uml-mcp.vercel.app/mcp](https://uml-mcp.vercel.app/mcp).

Claude Code entrypoint: **[CLAUDE.md](CLAUDE.md)**.
