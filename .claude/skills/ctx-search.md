---
name: ctx-search
description: "Search the project context index for business/technical tickets matching keywords. Greps CONTEXT_INDEX.md and returns matching tickets with key, title, and status. Test/QA-only tickets not indexed."
---

# ctx-search — Search Ticket Context

## When to use
User types `/ctx-search <query>` to find tickets relevant to current work.

## Flow

1. If no query provided, ask: "What would you like to search for? (keywords or ticket description)"
2. Spawn a subagent via the Agent tool (`subagent_type: "general-purpose"`, `description: "ctx-searcher"`, `run_in_background: false`). Pass as prompt:

```
You are a context searcher. Read agents/ctx-search.md for your instructions.

QUERY: <query>
```

3. The subagent returns matching tickets. Present results:

```
## Search Results for "<query>"

| Key | Title | Status |
|-----|-------|--------|
| V551-36 | Update inventory synchronization logic | done |

1-3 of 3 matches
```

4. If no matches, suggest: "No direct matches in CONTEXT_INDEX.md. You may also check `docs/architecture/` or `docs/srs/` for broader project documentation."
