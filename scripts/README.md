# uml-skill scripts

Small **stdlib-only** helpers agents can run without installing uml-mcp.

## `kroki_url.py`

Prints a **Kroki GET URL** for diagram source (zlib + base64url, same encoding as [Kroki](https://kroki.io/) expects).

**When to use:** MCP / `generate_uml` is unavailable and you only need a shareable URL.

**When to prefer uml-mcp:** The server maps **logical** `diagram_type` keys, themes, PlantUML preparation, validation, and fallbacks. This script takes a Kroki **backend** name (`plantuml`, `mermaid`, `d2`, …) and **raw** source—no uml-mcp wrapping.

### Usage

```bash
python kroki_url.py --help
```

From a file:

```bash
python kroki_url.py -t mermaid diagram.mmd
python kroki_url.py -t plantuml -f png diagram.puml
```

From stdin (PowerShell):

```powershell
Get-Content diagram.puml -Raw | python kroki_url.py -t plantuml
```

From stdin (Unix):

```bash
cat diagram.puml | python kroki_url.py -t plantuml
```

### Options

| Option | Meaning |
| --- | --- |
| `-t`, `--diagram-type` | Kroki backend path segment (default: `plantuml`) |
| `-f`, `--format` | Output format segment (default: `svg`) |
| `--base` | Kroki base URL (default: `https://kroki.io`) |

Positional `file` is optional; if omitted, diagram text is read from **stdin** (not from an interactive TTY).
