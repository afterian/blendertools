# Blender Tools

Local Python environment for connecting AI assistants (Claude, etc.) to Blender over the
[Model Context Protocol](https://modelcontextprotocol.io).

| Folder | Package | Source | Entry point |
|---|---|---|---|
| `blender-lab-mcp-venv/` | `blender-mcp` 1.0.2 (official Blender Lab) | [projects.blender.org/lab/blender_mcp](https://projects.blender.org/lab/blender_mcp) (`mcp/` subdir, commit `ff54e4d`) | `Scripts\blender-mcp.exe` |

Runs on Python 3.14.5 and talks to a Blender add-on over a local TCP socket
(default **`localhost:9876`**).

---

## Blender Lab MCP (`blender-lab-mcp-venv`)

The official Blender Foundation (Blender Lab) server. Focused on inspecting and driving Blender
itself, with built-in search over the Blender Python API and user manual.

Tools include: `execute_blender_code`, blend-file summaries (datablocks, linked libraries,
missing files, usage guess), object summaries, window/area screenshots, jumping to tabs/objects,
viewport and thumbnail renders, and `search_api_docs` / `search_manual_docs` / `get_python_api_docs`.

### Install the Blender add-on

The pip package is only the MCP server. The matching Blender add-on lives in the same
repository (`https://projects.blender.org/lab/blender_mcp`); install it in Blender via
**Edit → Preferences → Add-ons → Install from Disk** and enable it so it listens on port 9876.

### MCP client config

```json
{
  "mcpServers": {
    "blender-lab": {
      "command": "D:\\tools\\blender-lab-mcp-venv\\Scripts\\blender-mcp.exe"
    }
  }
}
```

### Options

| Setting | Purpose |
|---|---|
| `--transport stdio\|http` (`-t`) | Transport (default `stdio`) |
| `--host` | Bind host for HTTP transport (default `127.0.0.1`) |
| `--port` (`-p`) | Bind port for HTTP transport (default `8000`) |
| `BLENDER_MCP_HOST` | Blender add-on socket host (default `localhost`) |
| `BLENDER_MCP_PORT` | Blender add-on socket port (default `9876`) |

Run as a local HTTP server (e.g. for llama.cpp's web UI):

```bash
D:/tools/blender-lab-mcp-venv/Scripts/blender-mcp.exe --transport http --port 8000
```

---

## Adding to Claude Code

```bash
claude mcp add blender-lab -- D:/tools/blender-lab-mcp-venv/Scripts/blender-mcp.exe
```

## Updating

```bash
D:/tools/blender-lab-mcp-venv/Scripts/python.exe -m pip install -U "git+https://projects.blender.org/lab/blender_mcp.git#subdirectory=mcp"
```

## Troubleshooting

- **Server seems to hang when run by hand** — normal; stdio servers wait for an MCP client on stdin.
  Launch it from your MCP client instead.
- **"Could not connect to Blender"** — make sure Blender is open, the add-on is enabled and
  listening, and nothing else is holding port 9876.
- **Security note** — the server executes arbitrary Python inside your Blender process. Review
  code before approving it.
