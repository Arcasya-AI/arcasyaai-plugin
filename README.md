# ArcasyaAI MCP plugin

Public listing package so Cursor, Grok Bot, Claude, ChatGPT, and other MCP clients can install **ArcasyaAI MCP** without seeing the private product codebase.

Hosted MCP (already live):

```text
https://secure.arcasya.ai/api/mcp/mcp
```

Product, privacy, and docs: [arcasya.ai](https://arcasya.ai)

## What this repo is

- A thin client-facing wrapper: Cursor plugin manifest, Agent Plugins manifest, and `mcp.json` that point at the hosted MCP.
- Public GitHub packaging for marketplace / directory submissions.
- Brand assets (icon + wordmark) committed under `assets/`.

## What this repo is not

- **Not the ArcasyaAI product source.** Apps, engine, Supabase, and the private `arcasyaai-v3` codebase stay private.
- **Not a self-hosted MCP server.** There is no local MCP process to run.
- **Not a place for secrets.** Do not commit API keys, OAuth client secrets, or customer data.

If you need product source, you are in the wrong repository.

## Install

### Cursor (marketplace)

1. Submit / install from [cursor.com/marketplace](https://cursor.com/marketplace) once the listing is published ([publish form](https://cursor.com/marketplace/publish)).
2. Or clone this repo and load it locally from `~/.cursor/plugins/local/arcasyaai` (see [Cursor plugin docs](https://cursor.com/docs/plugins.md#test-plugins-locally)).
3. Connect with the **OAuth card** when Cursor prompts. That is the one-click path.

Optional: set `ARCASYA_API_KEY` under **Plugins → Configure** if your client cannot complete OAuth. The committed `mcp.json` is URL-only so marketplace OAuth is not blocked by an empty `Authorization` header.

### Grok Bot / Grok Build

- After this repo is public, open a catalog PR against [xai-org/plugin-marketplace](https://github.com/xai-org/plugin-marketplace) pinning a commit SHA of this repository.
- Inside Grok Build: `/marketplace` → install `arcasyaai`, or connect the hosted URL below.
- Grok discovers MCP from `mcp.json` via `.grok-plugin/plugin.json`.

### Claude

- Directory (Team/Enterprise): [Claude Connectors Directory submission](https://claude.ai/admin-settings/directory/submissions/new) — guidelines at [claude.com/docs/connectors/building/submission](https://claude.com/docs/connectors/building/submission).
- Manual: add a custom connector with URL `https://secure.arcasya.ai/api/mcp/mcp` and complete OAuth.

### ChatGPT / OpenAI plugins

- Submit the hosted MCP URL through the [OpenAI plugin submission portal](https://developers.openai.com/plugins/deploy/submission) (Apps dashboard: [platform.openai.com/apps-manage](https://platform.openai.com/apps-manage/)).
- Use the **universal** MCP URL above. Auth is OAuth 2.1 (PKCE) on `https://secure.arcasya.ai`.

### Manual MCP URL (any client)

OAuth (preferred):

```json
{
  "mcpServers": {
    "arcasyaai": {
      "url": "https://secure.arcasya.ai/api/mcp/mcp"
    }
  }
}
```

API key (optional, for clients without OAuth):

```json
{
  "mcpServers": {
    "arcasyaai": {
      "url": "https://secure.arcasya.ai/api/mcp/mcp",
      "headers": {
        "Authorization": "Bearer ${ARCASYA_API_KEY}"
      }
    }
  }
}
```

A copy of the API-key variant lives at [`docs/mcp.api-key.example.json`](docs/mcp.api-key.example.json). Do not put a real key in git.

## Authentication

### OAuth (one-click)

The hosted MCP advertises OAuth:

- Authorization server: https://secure.arcasya.ai/.well-known/oauth-authorization-server
- Protected resource: https://secure.arcasya.ai/.well-known/oauth-protected-resource/api/mcp/mcp

Clients that support MCP OAuth should show a connect card. No secrets belong in this repo.

### Mint an API key (optional Bearer)

1. Sign in to [Launchpad → Developers](https://secure.arcasya.ai/launchpad?settings=developers).
2. Create a personal / org API key.
3. Grant **only the scopes you need** (least privilege).
4. Store the key in the client’s secret store or plugin variable `ARCASYA_API_KEY`. Never commit it.

Published scopes (from the live authorization server):

| Scope | Typical use |
| --- | --- |
| `agents:read` | List installed agents |
| `agents:run` | Dispatch an agent |
| `runs:read` | Poll run status |
| `approvals:act` | Approve or reject held steps |
| `chat` | Chat with the master agent |
| `memory:read` | Search memory / contacts |
| `memory:write` | Write or extract memories |
| `memory:graph` | Traverse the memory graph |
| `webhooks:manage` | Manage webhooks |
| `tuskforce:read` | Tuskforce read |
| `tuskforce:draft` | Tuskforce draft |
| `tuskforce:chat` | Tuskforce chat |
| `social:read` | Social brands / posts (read) |
| `social:write` | Social drafts and updates |
| `social:publish` | Record that a human posted |
| `social:admin` | Social admin (superset) |

Send the key as `Authorization: Bearer <key>`.

## Layout

```text
arcasyaai-plugin/
├── .cursor-plugin/plugin.json   # Cursor Plugin (logo, variables, marketplace fields)
├── .grok-plugin/plugin.json     # Grok Build catalog helper
├── plugin.json                  # Agent Plugins portable manifest
├── mcp.json                     # Hosted Streamable HTTP MCP (OAuth / URL-only)
├── assets/logo.svg              # Marketplace icon
├── assets/wordmark.svg          # Optional wordmark
├── docs/                        # Server-card snapshot + API-key example
├── LICENSE                      # MIT
└── README.md
```

`mcp.json` is URL-only on purpose: Agent Plugins forbids secrets in headers, and an empty `Bearer ${ARCASYA_API_KEY}` header would break marketplace OAuth. Cursor still declares optional `ARCASYA_API_KEY` under plugin `variables`.

## Marketplace submission checklist

Use this repo’s public URL: `https://github.com/Arcasya-AI/arcasyaai-plugin`

- [ ] **Cursor:** [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish) — valid `.cursor-plugin/plugin.json`, committed `assets/logo.svg`, public repo, no secrets.
- [ ] **Grok Bot / Grok Build:** PR to [xai-org/plugin-marketplace](https://github.com/xai-org/plugin-marketplace) adding a remote entry that pins a 40-character commit SHA of this repo. Then `python3 scripts/generate-plugin-index.py` and `python3 scripts/validate-catalog.py` in that catalog repo.
- [ ] **Claude directory:** [claude.ai/admin-settings/directory/submissions/new](https://claude.ai/admin-settings/directory/submissions/new) (Team/Enterprise + directory permission). MCP URL `https://secure.arcasya.ai/api/mcp/mcp`.
- [ ] **OpenAI / ChatGPT plugins:** [developers.openai.com/plugins/deploy/submission](https://developers.openai.com/plugins/deploy/submission) — universal MCP URL, OAuth, privacy policy, logo.

Privacy policy: [arcasya.ai/privacy-policy](https://www.arcasya.ai/privacy-policy)  
Terms: [arcasya.ai/terms](https://www.arcasya.ai/terms)

## License

MIT. The listing package is public; **ArcasyaAI product source remains private.**
