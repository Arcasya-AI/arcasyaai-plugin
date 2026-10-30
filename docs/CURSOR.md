# Cursor

Dated 2026-10-30.

## Local load

Copy this repo to `~/.cursor/plugins/local/arcasyaai` and reload Cursor. The server name Cursor shows is `plugin-arcasyaai-arcasyaai`.

## Deeplink

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=arcasyaai&config=BASE64
```

`BASE64` is the standard base64 of `{"url":"https://secure.arcasya.ai/api/mcp/mcp"}`. The Launchpad Connect page builds that link for the current host. The marketplace plugin URL stays `https://secure.arcasya.ai/api/mcp/mcp`.

No `Authorization` header. Cursor runs OAuth.

## Submit

The listing is not published until this repository is submitted at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). That one listing is the door for Cursor, the CLI, Cloud Agents, and Grok Bot.

After install, type `@` to attach the connector and `/` for a skill.
