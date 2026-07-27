# ctx Format Contract

This document defines the file formats used by the ctx persistent context plugin.
**ALL subagents must read this before reading or writing context files.**

## Scope

This plugin archives **business and technical changes only**. Test-related content (test case comments, QA testing notes, fix-test tickets) is excluded to keep context lean. Archive only what informs future development decisions.

## Directory Tree

```
docs/
  CONTEXT_INDEX.md              ← ticket manifest
  TICKET_GRAPH.md               ← relationship edge list
  jira/
    <PROJECT-PREFIX>-<NUMBER>/
      TICKET.md                 ← structured frontmatter + document timeline
      <NNN>-<kebab-case-title>.md
  architecture/
  srs/
  brd/
  api/
  database/
  deployment/
  coding-guidelines/
```

## CONTEXT_INDEX.md Format

Pipe-separated table with header row. Four columns: Key, Title, Keywords, Status.

| Key | Title | Keywords | Status |
|-----|-------|----------|--------|

Status values: draft, in_progress, done, blocked, cancelled.

## TICKET_GRAPH.md Format

Heading-per-ticket as `**KEY: Title**`. Relationship edges as bullet list:
- relationship_type: target_key
- relationship_type: target_key1, target_key2

Valid relationship types: continues, depends_on, blocked_by, duplicates, split_from, merged_into, parent, child, related, implements, fixes, regression_of.

## TICKET.md Format

YAML frontmatter fields: key, title, status, keywords (array), created (date), updated (date).
Body sections: Description, Document Timeline (table), Related Tickets (bullets).

## Individual Document Format

YAML frontmatter fields: author, role (PM|BA|Developer|Architect), date, type, ticket.
type values: Requirement, Clarification, Design, Decision, Dev Note. (No testing/QA types.)
Filename: NNN-<kebab-case-title>.md where NNN is zero-padded sequence number.
