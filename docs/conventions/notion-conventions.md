# Notion Conventions

> Notion-flavored markdown and SR workspace conventions an agent must follow to
> write content that renders correctly and stays consistent with the workspace.

## Notion-flavored markdown

Notion extends standard Markdown with XML-like tags and attribute lists. The
hosted MCP reads and writes this dialect. The forms agents use most:

| Element | Syntax |
| --- | --- |
| Callout | `<callout icon="✨" color="purple_bg">text</callout>` |
| Toggle heading | `### Heading {toggle="true"}` with tab-indented children |
| Columns | `<columns><column>…</column><column>…</column></columns>` |
| Mention a page | `<mention-page url="https://www.notion.so/…"/>` |
| Mention a user | `<mention-user url="user://<id>"/>` |
| Mention a data source | `<mention-data-source url="collection://<id>"/>` |
| Coloured heading | `# Title {color="green"}` |
| Divider | `---` |

Rules:

- **Child blocks are nested by one tab** per level. Indentation is structural.
- **Cap headings at H3.** Deeper headings collapse to H3 on write.
- **Don't invent tags.** If you're unsure a block type exists, use a plain
  paragraph, list, or callout. Fetch `notion://docs/enhanced-markdown-spec` when
  you need the authoritative syntax.
- **Resolve database → data source IDs** before querying or writing. A database
  (`https://www.notion.so/<id>`) contains one or more data sources
  (`collection://<id>`); schema and queries operate on the data source.

## Writing-position discipline

Several SR agents write only into a bounded region of a page (e.g. Aliché writes
the Morning Sunshine dashboard *between the first two dividers* and never edits
below). Agents in this repo follow the same principle:

- **Write only where you're told.** Define the exact target region in the spec.
- **Never edit content you didn't create** unless the spec explicitly says to
  update a specific block.
- **Don't change database schemas.** Add rows/pages/comments; don't alter
  properties.

## Draft-only / sandboxed posture

For any agent with `write_posture: draft-only` (Hijack included):

- **Production databases:** read and **comment** only. Propose changes as
  comments via `notion-create-comment`, or as a draft block in the sandbox.
- **Sandbox page:** the one place the agent may freely create/edit. Name it in
  the spec.
- This is enforced structurally by Notion's per-resource permissions
  (Can view / Can comment / Can edit), not just by prose. Grant Can edit only on
  the sandbox. There is no delete tool, and version history makes edits
  reversible — so worst case is a recoverable edit.

## SR naming and structure conventions

- **Meetings** are named `Organisation or Participant + x EdR SR + > topic`.
- **Tasks** require Name, Project, Status, Priority, Timeline and link to a
  Project (or to a Deliverable, which rolls up to a Project — don't double-set).
- **Projects** require Name, Important, Urgent, Objective, Timeline, Lead.
- **Owners** are set only when a person is explicitly named and is a Notion user;
  otherwise leave empty.
- **Deal Flow** records carry Deal Stage, Owner, Sector, SIP bucket, Capital
  Type, Currency, and dated milestones; per-deal pages hold DOCUMENTS, DUE
  DILIGENCE (Dataroom, DDQ), and TIMELINE sections.
- **Timezone is Europe/Zurich** for all dates, schedules, and "today/this week".

## Rate limits and robustness

- Expect ~3 requests/second; on HTTP 429 honour `Retry-After` and back off. (We
  hit a 429 during initial recon — this is real.)
- Paginate large queries (≈100 rows/page); don't assume you can scan everything
  at once.
- Chunk long generated content into multiple appends rather than one giant
  block.
- Use `filter_properties` / `SELECT` only the columns you need to keep payloads
  small.

## Transparency

When an agent writes to a shared surface, it should make its authorship and
recency legible — e.g. a dated note ("Drafted by Hijack, 2026-05-29") — so
humans know what to review. This matches SR's *Good Practices in Using AI*
(transparency) and *How we work* (leave trails others can follow).
