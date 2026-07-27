# ctx — Persistent Context Plugin for Claude Code

Ticket-centric project context for Claude Code. Business/technical change history, relationship graphs, and context retrieval — from Markdown files in your repo.

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed

## Installation

### From GitHub (no clone)

```bash
claude plugin marketplace add https://github.com/vthnhtng/ctx
claude plugin install ctx@ctx-dev
```

The plugin is now available in any project.

### Or activate manually (single project)

Copy these files into your project:

- `.claude-plugin/`, `.claude/skills/`, `agents/`, `commands/`, `references/`

Then add to `.claude/settings.local.json`:

```json
{
  "enabledPlugins": {
    "ctx@ctx-dev": true
  }
}
```

Skills in `.claude/skills/` auto-load when working inside the project.

## What's Included

### Skills (slash commands)

| Command | What it does |
|---------|-------------|
| `/ctx-search <keywords>` | Search ticket index for relevant business/technical tickets |
| `/ctx-brief <ticket-key>` | Load full ticket context — requirements, decisions, evolution |
| `/ctx-related <ticket-key>` | Explore relationship graph and dependency chains |

### CLAUDE.md integration

The project's `CLAUDE.md` includes a persistent context section so Claude Code checks the ticket index before starting development work.

## Quick Start

1. Fill in historical context — see [Data Ingestion](#data-ingestion) below
2. Start a ticket: describe a new Jira ticket to Claude Code
3. Search: `/ctx-search <keywords from the new ticket>`
4. Brief: `/ctx-brief <relevant-ticket-key>` to understand past decisions
5. Relate: `/ctx-related <ticket-key>` to trace dependencies

## Data Ingestion

Context comes from Jira tickets. Use the provided prompt template with a Jira AI agent:

1. Open `references/jira-agent-prompt.md`
2. Replace `{{TICKET_KEY}}` with your ticket key
3. Paste into your Jira agent
4. Agent outputs scaffold commands + file contents
5. Run the commands from the project root, then copy file contents

**Scope:** Business and technical changes only. Test/QA/fix-test tickets are excluded.

## Plugin Structure

```
.claude-plugin/plugin.json        — plugin metadata
.claude/skills/ctx-search.md       — /ctx-search skill definition
.claude/skills/ctx-brief.md        — /ctx-brief skill definition
.claude/skills/ctx-related.md      — /ctx-related skill definition
agents/ctx-search.md               — search subagent
agents/ctx-brief.md                — brief subagent
agents/ctx-related.md              — relate subagent
references/ctx-format.md           — format contract (all subagents read this)
references/jira-agent-prompt.md    — Jira agent prompt template
commands/                          — skills duplicated for plugin discovery
docs/CONTEXT_INDEX.md              — ticket manifest
docs/TICKET_GRAPH.md               — relationship graph
docs/jira/<key>/                   — per-ticket docs
```
