# uCoz MCP

## Quick start

1. In your AI client MCP settings, add:

```json
{
  "mcpServers": {
    "ucoz-mcp": {
      "url": "https://www.ucoz.com/mcp"
    }
  }
}
```

2. Authorize through the uCoz Control Panel when prompted.
3. Ask something like: “List my sites and show active modules on the first one.”

More clients: [`clients/`](clients/). Step-by-step: [`docs/connect.md`](docs/connect.md).

## What this is

Remote MCP for uCoz: connect Cursor, Claude, ChatGPT/Codex, VS Code, and other MCP clients to your account — pick or create a site, then manage templates, content, shop, users, and site files.

- **MCP URL:** `https://www.ucoz.com/mcp`
- **Transport:** Remote HTTP
- **Auth:** Control Panel sign-in (no API key in client config)
- **Product page:** [ucoz.com/ai/mcp](https://www.ucoz.com/ai/mcp)
- **Registry metadata:** [`server.json`](server.json)

## What the agent can do

After Control Panel authorization: account tools (`list_sites`, `select_site`, `create_site`) and site tools — templates and pages, Template Maker, content modules, shop, subscriptions, users, modules/quarantine, site files (`files_tool`), FTP password management only (`ftp_tool`).

Full list: [`docs/tools.md`](docs/tools.md). Example prompts: [`docs/prompts.md`](docs/prompts.md).

## Agent Skills

Official playbooks live in **[ucoz-skills/agent-skills](https://github.com/ucoz-skills/agent-skills)** (also IDE plugins). This repository is the MCP connect / catalog surface.

## Related links

- Product: https://www.ucoz.com/ai/mcp
- API docs: https://api.ucoz.net/en/mcp.html
- Skills + plugins: https://github.com/ucoz-skills/agent-skills
- Official MCP Registry: https://registry.modelcontextprotocol.io/
- Security: [SECURITY.md](SECURITY.md)

## License

MIT — see [LICENSE](LICENSE). Applies to this repository (docs, configs, registry metadata). The uCoz platform is provided under uCoz terms of service.