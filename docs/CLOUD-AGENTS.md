# Cloud Agents

Dated 2026-10-30.

`mcp.json` uses `type: streamable-http` because that is the Agent Plugins transport. The Cloud Agents API uses `type: http` for the same URL. Do not put `http` in `mcp.json`.

```http
POST https://api.cursor.com/v1/agents
```

```json
{
  "mcpServers": [
    {
      "name": "arcasyaai",
      "type": "http",
      "url": "https://secure.arcasya.ai/api/mcp/mcp"
    }
  ]
}
```

Prefer OAuth `auth` over a baked `Authorization` header. The product does not ship a static OAuth client id. A URL with no header lets the agent discover OAuth.

Auth for the Cloud Agents API itself is a user key or a service-account key whose prefix is `crsr_`. A `grok_` key returns 403.

- At most 50 MCP servers.
- A steer follow-up cannot change `mcpServers`.
- Back off on HTTP 429.

Admin, Analytics, AI Code Tracking, and Bugbot are Enterprise usage APIs. They are not this plugin.
