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
4. Type `@` to attach the connector and `/` to run a skill. The skill to hand work to Launchpad is `run-launchpad`.

Local load, the install deeplink, and the still-open submit step: [docs/CURSOR.md](docs/CURSOR.md).

The committed `mcp.json` is URL-only so marketplace OAuth stays free of an empty `Authorization` header. Grok Bot connects with OAuth. The Cursor manifest has no API-key setting.

### Grok Bot / Grok Build

Grok Bot installs this plugin from the Cursor Marketplace. Submit the listing at [https://cursor.com/marketplace/publish](https://cursor.com/marketplace/publish), then connect with OAuth.

- Inside Grok Build: `/marketplace` → install `arcasyaai`, or connect the hosted URL below.
- Grok discovers MCP from `mcp.json` via `.grok-plugin/plugin.json`.
- After install, type `@` to attach the connector and `/` for a skill. Connectors are account-wide. Team allow and block is Dashboard → Plugins.

Full notes: [docs/GROK-BOT.md](docs/GROK-BOT.md). Profile sync inside Launchpad is a different product and is not this plugin.

### Claude

- Directory (Team/Enterprise): [Claude Connectors Directory submission](https://claude.ai/admin-settings/directory/submissions/new) — guidelines at [claude.com/docs/connectors/building/submission](https://claude.com/docs/connectors/building/submission).
- Manual: add a custom connector with URL `https://secure.arcasya.ai/api/mcp/mcp` and complete OAuth.

### ChatGPT / OpenAI plugins

- Submit the hosted MCP URL through the [OpenAI plugin submission portal](https://developers.openai.com/plugins/deploy/submission) (Apps dashboard: [platform.openai.com/apps-manage](https://platform.openai.com/apps-manage/)).
- Use the **universal** MCP URL above. Auth is OAuth 2.1 (PKCE) on `https://secure.arcasya.ai`.
- If a host cannot speak MCP, the product ships a Custom GPT Actions schema as a fallback. Do not drop this MCP submission path.

Cloud Agents attach the same URL with `type: http` (not the `streamable-http` value in `mcp.json`). See [docs/CLOUD-AGENTS.md](docs/CLOUD-AGENTS.md).

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

#### Advanced: API key

Advanced, for clients that cannot complete OAuth. Cursor and Grok Bot connect with OAuth. A copy lives at [`docs/mcp.api-key.example.json`](docs/mcp.api-key.example.json). Do not put a real key in git.

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

## Authentication

### OAuth (one-click)

The hosted MCP advertises OAuth:

- Authorization server: https://secure.arcasya.ai/.well-known/oauth-authorization-server
- Protected resource: https://secure.arcasya.ai/.well-known/oauth-protected-resource/api/mcp/mcp

Clients that support MCP OAuth should show a connect card. No secrets belong in this repo.

### Scopes and public tools

The hosted server hides internal tools from public users ([arcasyaai-v3#219](https://github.com/Arcasya-AI/arcasyaai-v3/pull/219)). A new public OAuth client that omits `scope` receives the default public scopes below. The live authorization server still advertises a broader `scopes_supported` list. Public clients receive the default grant.

Default public scopes:

| Scope | Typical use |
| --- | --- |
| `agents:read` | List installed agents |
| `agents:run` | Dispatch an agent |
| `runs:read` | Poll run status |
| `chat` | Chat with the master agent, and `ask_<persona>` |
| `memory:read` | Search memory |
| `memory:write` | Write or extract memories |
| `matrix:read` | Intelligence Matrix overview, search, table, and graph |
| `matrix:write` | Typed matrix context, links, and project hubs |
| `social:read` | Read social brands, posts, and reports |
| `crm:read` | Read CRM clients, contacts, notes, and issues |

`approvals:act` is not in that default. It is opt-in. An OAuth caller can reject a hold. It cannot approve one. A person approves in Launchpad. See [docs/SCOPES.md](docs/SCOPES.md).

Public tools on that grant:

- `chat`
- `ask_<persona>` (one tool per persona in the caller’s org, when `chat` is granted)
- agents / runs
- memory
- Intelligence Matrix (`matrix_overview`, `matrix_search`, `matrix_get_node`, `matrix_traverse`, `matrix_list`, `matrix_write`, `matrix_update`, `matrix_link`)
- contacts
- read-only CRM
- read-only social

Matrix tools stay inside the connected org. Organization-scope writes land pending review. A new project hub is created only for an owner or admin.

The same tools can render MCP App cards (`ui://arcasya/matrix-explorer.html`, `ui://arcasya/approval-card.html`, `ui://arcasya/run-status.html`) when the host supports `io.modelcontextprotocol/ui`. Hosts that do not still receive the normal text and structured tool result.

Admin scopes and white-label scopes are **internal only**. They are not granted to public clients. The server also hides the tools behind them from public users, including when a token lists a broader scope set.

| Scope | Why it is internal |
| --- | --- |
| `crm:admin` | Admin. Not granted to public clients. |
| `social:admin` | Admin. Not granted to public clients. |
| `tuskforce:read` | White-label. Not granted to public clients. |
| `tuskforce:draft` | White-label. Not granted to public clients. |
| `tuskforce:chat` | White-label. Not granted to public clients. |

### Advanced: mint an API key

Optional Bearer auth for clients without OAuth. Cursor and Grok Bot connect with OAuth and do not use this setting.

1. Sign in to [Launchpad → Connect](https://secure.arcasya.ai/launchpad/mcp). A key created there defaults to the public scopes above. The wide bundle is opt-in and listed.
2. Grant **only the scopes you need** (least privilege).
3. Store the key in the client’s secret store. Never commit it.

Send the key as `Authorization: Bearer <key>`.

## Layout

```text
arcasyaai-plugin/
├── .cursor-plugin/plugin.json   # Cursor Plugin (logo, marketplace fields). OAuth only.
├── .grok-plugin/plugin.json     # Grok Build catalog helper
├── plugin.json                  # Agent Plugins portable manifest
├── mcp.json                     # Hosted Streamable HTTP MCP (OAuth / URL-only)
├── assets/logo.svg              # Marketplace icon
├── assets/wordmark.svg          # Optional wordmark
├── skills/run-launchpad/SKILL.md
├── skills/launchpad-approvals/SKILL.md
├── rules/arcasya-layer.mdc
├── commands/ask-arcasya.md
├── docs/                        # Grok Bot, Cursor, Cloud Agents, scopes
├── LICENSE                      # MIT
└── README.md
```

Scope lists, dated: [docs/SCOPES.md](docs/SCOPES.md). If that file and the live server disagree, the server wins.

`mcp.json` is URL-only on purpose: Agent Plugins forbids secrets in headers, and an empty `Bearer ${ARCASYA_API_KEY}` header would break marketplace OAuth. The Cursor manifest has no `ARCASYA_API_KEY` setting.

## Marketplace submission checklist

Use this repo’s public URL: `https://github.com/Arcasya-AI/arcasyaai-plugin`

- [ ] **Cursor:** [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish) — submit this repository. Package notes: [docs/CURSOR.md](docs/CURSOR.md). Valid `.cursor-plugin/plugin.json`, committed `assets/logo.svg`, public repo, no secrets.
- [ ] **Grok Bot:** same Cursor listing. Install notes: [docs/GROK-BOT.md](docs/GROK-BOT.md).
- [ ] **Claude directory:** [claude.ai/admin-settings/directory/submissions/new](https://claude.ai/admin-settings/directory/submissions/new) (Team/Enterprise + directory permission). MCP URL `https://secure.arcasya.ai/api/mcp/mcp`.
- [ ] **OpenAI / ChatGPT plugins:** [developers.openai.com/plugins/deploy/submission](https://developers.openai.com/plugins/deploy/submission) — universal MCP URL, OAuth, privacy policy, logo. Actions in the product is only the fallback.

Privacy policy: [arcasya.ai/privacy-policy](https://www.arcasya.ai/privacy-policy)  
Terms: [arcasya.ai/terms](https://www.arcasya.ai/terms)

## License

MIT. The listing package is public; **ArcasyaAI product source remains private.**
