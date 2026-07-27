---
name: ctx-brief
description: "Load a ticket's business/technical context — reads TICKET.md and timeline documents, returns a chronological summary of requirements, design decisions, and evolution. Test/QA content excluded."
---

# ctx-brief — Ticket Context Brief

## When to use
User types `/ctx-brief <ticket-key>` to get a comprehensive understanding of a ticket's history.

## Flow

1. If no ticket key provided, ask: "Which ticket key? (e.g., V551-36)"
2. Validate format: `<project-prefix>-<number>`. If invalid, ask again.
3. Spawn a subagent via the Agent tool (`subagent_type: "general-purpose"`, `description: "ctx-briefer"`, `run_in_background: false`). Pass as prompt:

```
You are a context briefer. Read agents/ctx-brief.md for your instructions.

TICKET_KEY: <ticket-key>
```

4. Present the result to the user as-is. The subagent returns a formatted briefing.
5. If subagent reports "not found", tell the user: "Ticket `<key>` not found in docs/jira/. Make sure the documents have been added."
