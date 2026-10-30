# Scopes

Dated 2026-10-30. Copied from the product grant in `selectOAuthGrantScopes`. If this file and the live server disagree, the server wins.

The hosted well-known `scopes_supported` list is wider than the default grant. An omitted public `scope` receives only the default list.

## Default public grant

- `agents:read`
- `agents:run`
- `runs:read`
- `chat`
- `memory:read`
- `memory:write`
- `matrix:read`
- `matrix:write`
- `social:read`
- `crm:read`

Tools: `chat`, `ask_<persona>`, agents and runs, memory, matrix, contacts, read-only CRM, read-only social.

## Opt-in

A public client must request these. An omitted grant does not include them.

- `approvals:act` — OAuth may reject a hold. It may not approve an action gate, and it may not approve a hold it created. A person approves in Launchpad.
- `floor:control` — a person still decides Floor cards.
- `rooms:read`
- `rooms:write`
- `workflows:read`
- `workflows:run`
- `memory:graph`

Finance and sales scopes stay opt-in the same way. They are not part of the marketplace body.

## Withheld

Not on the public plugin, even if requested:

- Embodiment (`personas:embody` and charter tools)
- `office:control`
- GitHub and Notion write
- Social publish
- Admin
- Brand scopes

`tuskforce:read`, `tuskforce:draft`, and `tuskforce:chat` stay internal. They are not granted to a public client.
