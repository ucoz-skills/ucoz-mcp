# Connect to uCoz MCP

## Endpoint

```
https://www.ucoz.com/mcp
```

Transport: remote HTTP (Streamable HTTP).

Auth: sign in through the uCoz Control Panel when your AI client opens the MCP connection. No API key in the client config.

After auth, the agent typically calls `list_sites` → `select_site` (or `create_site`) before site tools.

## Cursor

Add to MCP config (project `.cursor/mcp.json` or global `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ucoz-mcp": {
      "url": "https://www.ucoz.com/mcp"
    }
  }
}
```

Or copy [`clients/cursor.json`](../clients/cursor.json). Complete Control Panel authorization when prompted.

## Claude Desktop / Claude Code

See [`clients/claude-desktop.json`](../clients/claude-desktop.json).

## VS Code / other clients

See [`clients/`](../clients/).

## Agent Skills

https://github.com/ucoz-skills/agent-skills

## Product page

https://www.ucoz.com/ai/mcp