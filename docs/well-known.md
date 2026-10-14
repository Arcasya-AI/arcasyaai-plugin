# Live well-known (hosted)

This repository is a listing wrapper. Discovery metadata is published on the hosted MCP origin, not from GitHub Pages.

| Resource | Live URL |
| --- | --- |
| MCP endpoint | https://secure.arcasya.ai/api/mcp/mcp |
| OAuth authorization server | https://secure.arcasya.ai/.well-known/oauth-authorization-server |
| OAuth protected resource | https://secure.arcasya.ai/.well-known/oauth-protected-resource/api/mcp/mcp |
| Developer settings (API keys) | https://secure.arcasya.ai/launchpad?settings=developers |

`docs/server-card.json` is a **documentation snapshot** of that hosted card. It records the default public scopes, the internal-only admin and white-label scopes, and the public tool list. The live authorization server still advertises the full `scopes_supported` list; a new public client that omits `scope` receives only the public default. If the live metadata and this snapshot disagree, the hosted files win.
