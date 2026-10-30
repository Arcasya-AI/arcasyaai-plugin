---
name: launchpad-approvals
description: Read Launchpad approval holds when this connection includes approvals:act. Use when the user asks what is waiting. OAuth may reject a hold. A person approves in Launchpad.
---

# Launchpad approvals

Use this only when the connection opted into `approvals:act`. An omitted public grant does not include it.

1. Call `list_pending_approvals`.
2. Report the holds. Do not invent a status.
3. OAuth `decide_approval` may reject a hold.
4. It may not approve an action gate.
5. It may not approve a hold this connection created.
6. A person approves in Launchpad.

Never claim a send, post, payment, or delete happened because a hold exists. Wait until a tool result says a person approved it.
