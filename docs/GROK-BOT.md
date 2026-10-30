# Grok Bot

Dated 2026-10-30. Grok Bot runs on a Cursor cloud computer. This plugin is the connector. There is no second Grok Bot connector list.

1. Add **ArcasyaAI** from the Cursor Marketplace, or install this repo.
2. Finish OAuth in the browser. The token stays on Cursor's connector backend. The Bot does not see it.
3. Type `@` to attach the connector.
4. Type `/` and choose `run-launchpad` or `launchpad-approvals`.

Installed connectors are account-wide. Every Bot on that computer shares them.

Team admins allow or block plugins from Dashboard → Plugins. Enterprise can enforce an MCP allowlist. Auto Review evaluates a plugin call before it runs and can allow, ask, or deny.

`mcp.json` is URL only: `https://secure.arcasya.ai/api/mcp/mcp`. No API key.

Profile sync at `/launchpad/sync/grok-bot` walks a shareable profile into Launchpad. That is a different product. This plugin does not replace it.

The slim chat-session server at `/api/mcp/grok-bot/mcp` is also a different door. This plugin points at the platform server.
