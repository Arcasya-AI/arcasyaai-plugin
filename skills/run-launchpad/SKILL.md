---
name: run-launchpad
description: Hand a Launchpad job to Arcasya when the user wants chat, an agent run, memory, the Intelligence Matrix, contacts, or read-only CRM and social. Use when the work should run in Launchpad and come back as a tool result.
---

# Run Launchpad

Use this when the user wants Arcasya to do the work and return a tool result. Attach the Arcasya connector with `@` first.

Teach only the default public grant. These tools are present when the client omitted `scope`:

- `chat` — optional `agent` (`@handle`, slug, or id). This is how you call a persona. The platform runs it.
- `ask_<persona>` — one tool per persona, when `chat` is granted.
- `list_agents`, `run_agent`
- `get_run_status`, `wait_for_run`
- `search_memory`, `write_memory`
- Matrix reads and writes
- `list_contacts`
- Read-only CRM and read-only social

`run_agent` returns a `run_id`. Poll with `get_run_status` or `wait_for_run`.

## Only when those scopes were requested

An omitted public grant does not include these. Use them only if the connection asked for the scope.

- `floor:control` — `floor_join`, `floor_post_card`, `floor_show_work`, `floor_wait`. A person decides the card. Do not claim it was approved unless `floor_wait` returns `resolution.action_kind` of `approve`.
- `rooms:read` and `rooms:write` — list, read, and post in a room.
- `workflows:read` and `workflows:run` — list and run a workflow.
- `memory:graph` — `traverse_memory_graph`.

## Stay inside the grant

Do not call embodiment, office, GitHub or Notion write, social publish, admin, or brand tools. They are not on this plugin.

Never send, post, pay, or delete unless a tool result says a human approved it.

If a field is missing from a tool result, say it is missing. Repeat the user's data exactly.
