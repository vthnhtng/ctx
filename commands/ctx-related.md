---
name: ctx-related
description: "Traverse the ticket relationship graph from a starting ticket. Walks up to 2 levels deep and returns connected business/technical tickets with relationship types."
---

# ctx-related — Ticket Relationship Explorer

## When to use
User types `/ctx-related <ticket-key>` to find related tickets and understand dependency chains.

## Flow

1. If no ticket key provided, ask: "Which ticket key? (e.g., V551-36)"
2. Spawn a subagent via the Agent tool (`subagent_type: "general-purpose"`, `description: "ctx-relater"`, `run_in_background: false`). Pass as prompt:

```
You are a relationship explorer. Read agents/ctx-related.md for your instructions.

TICKET_KEY: <ticket-key>
```

3. Present the result to the user. The subagent returns a formatted graph traversal.
4. If subagent reports "not found", tell the user: "Ticket `<key>` not found in TICKET_GRAPH.md."
