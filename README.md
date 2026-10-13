# Dolphin Industrial Parts MCP Server

A hosted, read-only [Model Context Protocol](https://modelcontextprotocol.io) server that lets AI agents look up the OEM brands, models and part numbers supported by [Dolphin Industrial Parts](https://dolphinindustrialparts.com) for compatible marine and industrial spare parts.

No prices and no stock levels: availability and price are quoted per application.

- **Endpoint:** `https://dolphinindustrialparts.com/agent/mcp`
- **Transport:** Streamable HTTP, stateless, JSON responses
- **Auth:** none
- **Registry name:** `com.dolphinindustrialparts/parts` ([official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=dolphinindustrialparts))
- **Server card:** https://dolphinindustrialparts.com/agent/mcp/server-card
- **Docs:** https://dolphinindustrialparts.com/for-ai-agents/

> This repository is documentation only. The server is a hosted service; there is nothing to install or run.

## Tools

All tools are read-only.

| Tool | What it does |
| --- | --- |
| `lookup_part` | Look up a part number (exact, then prefix, then a format-based brand and family guess) and get how to order. |
| `list_brands` | List the OEM brands and equipment families covered, with model counts. |
| `list_models` | List the equipment models and series covered for one OEM brand (for example IMO, Moatti, Alfa Laval). |
| `list_catalog` | Page through catalog part numbers and descriptions for one equipment family, 100 per page. |
| `request_quote` | Prepare a quote request (parts, quantities, equipment details). It never submits anything; it returns the form URL and contact details. |
| `get_manifest` | Site, contact details, endpoints, tools, limits and disclaimer. |

## Connect a client

### Claude Code

```sh
claude mcp add --transport http dolphin-parts https://dolphinindustrialparts.com/agent/mcp
```

Or use the `.mcp.json` in this repository.

### Claude Desktop and claude.ai

Settings > Connectors > Add custom connector, paste `https://dolphinindustrialparts.com/agent/mcp`, and choose No sign in.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "dolphin-parts": {
      "url": "https://dolphinindustrialparts.com/agent/mcp"
    }
  }
}
```

### Cline

Add to `cline_mcp_settings.json` (MCP Servers > Configure):

```json
{
  "mcpServers": {
    "dolphin-parts": {
      "type": "streamableHttp",
      "url": "https://dolphinindustrialparts.com/agent/mcp"
    }
  }
}
```

No API key or install step is needed.

### curl

```sh
curl -X POST https://dolphinindustrialparts.com/agent/mcp \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"list_models","arguments":{"brand":"IMO"}}}'
```

## Files

- `server.json`: the entry published to the official MCP Registry.
- `.well-known/mcp/server-card.json`: the server card served at https://dolphinindustrialparts.com/.well-known/mcp/server-card.json.
- `.mcp.json`: a ready-made project config for MCP clients.

## About

Dolphin Industrial Parts is a division of Dolphin Marine Services Inc., dba Dolphin Centrifuge, Warren, Michigan.

Contact: sales@dolphincentrifuge.com, (248) 522-2573.

## Disclaimer

Parts supplied by Dolphin Industrial Parts are compatible alternatives, not OEM parts. Dolphin Industrial Parts is not affiliated with, or authorized by, the equipment manufacturers named. Brand names, model names and part numbers are used only to identify compatibility.

## License

MIT. See [LICENSE](LICENSE).
