---
name: ask-arcasya
description: Ask Arcasya one question through the chat MCP tool.
---

# Ask Arcasya

1. Attach the Arcasya connector if it is not already attached.
2. Call `chat` once with the user's message.
3. Return the tool result. Do not add a send, booking, or payment that the result does not confirm.
4. If `chat` is missing, say the connector is not attached.
