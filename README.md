# uml-skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Smithery: diagramming-uml](https://img.shields.io/badge/Smithery-diagramming--uml-blue)](https://smithery.ai/skills/antoinebou12/uml)
[![Skill](https://img.shields.io/badge/skill-diagramming--uml-6f42c1)](SKILL.md)
[![uml-mcp](https://img.shields.io/badge/repo-uml--mcp-181717?logo=github)](https://github.com/antoinebou12/uml-mcp)
[![Kroki](https://img.shields.io/badge/renderer-Kroki-3B82F6)](https://kroki.io/)
[![Validate Smithery URL](https://github.com/antoinebou12/uml-skill/actions/workflows/validate-smithery-url.yml/badge.svg?branch=main)](https://github.com/antoinebou12/uml-skill/actions/workflows/validate-smithery-url.yml)

**uml-skill** is an agent skill bundle for **text-based UML and diagrams** (PlantUML, Mermaid, D2, BPMN, and more). When the **[uml-mcp](https://github.com/antoinebou12/uml-mcp)** server is connected, agents should call **`generate_uml`** / **`validate_uml`** to return **Kroki-backed URLs** (and optional playground links). Without MCP, you can still build a Kroki GET URL locally with **[scripts/kroki_url.py](scripts/kroki_url.py)** (see [scripts/README.md](scripts/README.md)).

**Canonical Smithery listing:** [diagramming-uml on Smithery](https://smithery.ai/skills/antoinebou12/uml) (`antoinebou12/uml`).

The full instructions live in **[SKILL.md](SKILL.md)** (`name: diagramming-uml`).

## Install

| Route | What to do |
| --- | --- |
| **Smithery (recommended for many agents)** | Run `npx @smithery/cli@latest skill add antoinebou12/uml`. Open the [Smithery skill page](https://smithery.ai/skills/antoinebou12/uml) and use **Install** for client-specific steps (**Cursor**, **Claude Code**, **Codex**, **Windsurf**, **Continue**, **GitHub Copilot**, and others). |
| **Cursor (manual)** | Copy this repository (or the `uml-skill` folder) into **`.cursor/skills/diagramming-uml/`** (or add a symlink). See [Cursor · Rules & skills](https://docs.cursor.com/context/rules-for-ai). |
| **Git clone / submodule** | `git clone https://github.com/antoinebou12/uml-skill.git` — then register **`SKILL.md`** or the whole folder according to your agent’s docs. |
| **Other clients** | Prefer the Smithery CLI and your product’s skill directory; paths and filenames vary. |

## Contents

- **[SKILL.md](SKILL.md)** — `diagramming-uml` skill (YAML frontmatter + workflow for uml-mcp / Kroki).
- **[scripts/](scripts/)** — [kroki_url.py](scripts/kroki_url.py) (stdlib Kroki URL helper) and [scripts/README.md](scripts/README.md).

## Related

- **[uml-mcp](https://github.com/antoinebou12/uml-mcp)** — MCP server with `generate_uml`, `validate_uml`, and `uml://*` resources.
- **Live MCP (deployed):** [https://uml-mcp.vercel.app/mcp](https://uml-mcp.vercel.app/mcp) (from the uml-mcp project).

## License

[MIT](LICENSE).
