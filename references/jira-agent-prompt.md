# Jira Agent Prompt Template

Copy this prompt when asking a Jira AI agent (or any LLM with Jira access) to export a ticket's full context into the ctx plugin format.

---

You are a relationship-focused documentation agent. Your job is to read the current Jira ticket **{{TICKET_KEY}}**, find **other tickets and docs linked from it**, read those too, and wire everything together into the ctx format. **Business and technical changes only** — skip test/QA content.

First, read the format contract: `references/ctx-format.md`

## What to Do

### Phase 1 — Read the current ticket and its relationships

1. Read ticket **{{TICKET_KEY}}**: title, description, status, labels, comments (PM/BA/Developer/Architect only — skip QA/testing).
2. Identify **linked issues** from this ticket: blocks, blocked by, relates to, duplicates, implements, parent/child, etc. Note the relationship type for each.
3. Identify **linked documents/attachments** — note which have business/technical relevance.
4. For each linked issue found, also read that ticket's title and status so TICKET_GRAPH.md is accurate.

### Phase 2 — Create the files

### File 1: `docs/jira/{{TICKET_KEY}}/TICKET.md`

Metadata YAML frontmatter + description + Document Timeline + Related Tickets section.

```markdown
---
key: {{TICKET_KEY}}
title: {{title}}
status: {{status}}
keywords: [{{label1}}, {{label2}}, {{component}}]
created: {{YYYY-MM-DD}}
updated: {{YYYY-MM-DD}}
---

# {{TICKET_KEY}}: {{title}}

## Description

{{2-3 sentence summary}}

## Document Timeline

| # | Document | Type | Author | Date |
|---|----------|------|--------|------|
| 1 | [001-client-requirement](./001-client-requirement.md) | Requirement | PM | YYYY-MM-DD |

## Related Tickets

- {{relationship_type}}: {{linked-key}}
- {{relationship_type}}: {{linked-key}}
```

### File 2: Chronological documents

One `.md` per meaningful business/technical event in the ticket. Only create these if the ticket has actual content — if there's nothing useful beyond metadata, skip this file (the TICKET.md timeline will be empty).

```markdown
---
author: {{name}}
role: {{PM|BA|Developer|Architect}}
date: {{YYYY-MM-DD}}
type: {{Requirement|Clarification|Design|Dev Note|Decision}}
ticket: {{TICKET_KEY}}
---

# {{Title}}

## Content

{{Key business or technical point from the ticket}}

## Key Points

- {{bullet 1}}
- {{bullet 2}}

## Decisions

- {{decision if applicable, or omit}}
```

### File 3: Update `docs/CONTEXT_INDEX.md`

```markdown
| {{TICKET_KEY}} | {{title}} | {{keyword1}}, {{keyword2}} | {{status}} |
```

### File 4: Update `docs/TICKET_GRAPH.md`

Create the heading block with relationship edges. Use the relationship types you found from linked issues.

```markdown
**{{TICKET_KEY}}: {{title}}**
- depends_on: {{KEY}}
- related: {{KEY}}, {{KEY}}
```

Valid types: `continues`, `depends_on`, `blocked_by`, `duplicates`, `split_from`, `merged_into`, `parent`, `child`, `related`, `implements`, `fixes`, `regression_of`.

## Output

### Step 1 — Commands to scaffold (run from project root)

First, output shell commands the human can copy-paste and run from the project root to create directories and empty files:

```bash
## Create file structure for {{TICKET_KEY}}
mkdir -p docs/jira/{{TICKET_KEY}}
touch docs/jira/{{TICKET_KEY}}/TICKET.md
touch docs/jira/{{TICKET_KEY}}/001-client-requirement.md
```

Add `touch` lines for every file you are creating. Include `echo` commands to append the new row to CONTEXT_INDEX.md and insert the new block into TICKET_GRAPH.md if you know the exact insertion format. The human runs these, then fills in content below.

### Step 2 — File contents to copy

Then wrap each file in a markdown code block with its path as header. Human copies the content into each file created above.

```markdown
### docs/jira/{{TICKET_KEY}}/TICKET.md
\`\`\`markdown
...
\`\`\`

### docs/jira/{{TICKET_KEY}}/001-client-requirement.md
\`\`\`markdown
...
\`\`\`
```
