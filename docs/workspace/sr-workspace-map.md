# SR Workspace Map

> Reference map of the Systemic Regeneration (SR) Notion workspace that the
> agents operate on. Captured 2026-05-29 via read-only recon. IDs are stable;
> the surrounding content is not. Re-verify before hardcoding anything.

SR is an Edmond de Rothschild (EdR) initiative working at the intersection of
capital, science, and living systems: a multi-capital investment strategy, a
proprietary intelligence library, a natural-capital advisory practice, and a
community of aligned actors. The flagship pilot is the Ferme des 30 Arpents
near Geneva. Workspace language mixes English and French; timezone is
**Europe/Zurich**.

## Teamspaces

| Teamspace | ID | Notes |
| --- | --- | --- |
| HQ | `1a6629c1-1208-8153-89c8-0042addff5a8` | Core team home; primary working space |
| Community | `206629c1-1208-8100-bd64-0042a5ff6aba` | External / aligned-actors space |

Connected sources observed: **Slack** (`systemicregeneration.slack.com`),
**Google Drive**, plus the **Calendar** and **Gmail** MCP endpoints attached to
this repo.

## People

Hijack's principal is **Arthur Garaud**. Other members are listed so the agent
can attribute owners and route correctly.

| Person | Role | Notion user ID | Email |
| --- | --- | --- | --- |
| Arthur Garaud | Co-Founder — investment, venture/PE pipeline, deal sourcing, DD, portfolio | `1a6d872b-594c-81d8-86ac-00024749add9` | arthur.garaud@edr-sr.com |
| Alice de Rothschild | Co-Founder — strategy, positioning | `1a6d872b-594c-8137-b7ed-000277c330c6` | alice.derothschild@edr-sr.com |
| Clarisse Hillou | Investment, pilots, portfolio (M&A / PE) | `209d872b-594c-816f-9ab1-000228bed555` | clarisse.hillou@edr-sr.com |
| Rita Sarkis | Science & Research | `1b4d872b-594c-8101-bd44-00023ea9470f` | rita.sarkis@edr-sr.com |
| Henry Morgan | Data / quant, sustainable finance | `311d872b-594c-81b8-8bed-0002a9a9eb4c` | henry.morgan@edr-sr.com |
| Jilly Yin | Brand & communications | `2dfd872b-594c-81f8-9876-000296948a31` | jilly.yin@edr-sr.com |
| Matthieu Bleuse | Data infrastructure & platform | _not captured_ | matthieu.bleuse@edr-sr.com |

## Key pages and databases

Hijack reads from these. Treat all production databases as **read / comment
only** (see the draft-only posture in the Hijack spec).

| Surface | Type | ID | Purpose |
| --- | --- | --- | --- |
| Team Home | page | `1a6629c1-1208-81df-a270-d7b564d7c4e3` | Org home base, team bios, operating info |
| How we work | page | `34e629c1-1208-81f6-8a5f-e57d462f71d4` | Culture + operating guidelines (see below) |
| Good Practices in Using AI | page | `344629c1-1208-8008-a266-db517119f539` | 8 responsible-AI principles (see below) |
| Notion Agents (parent) | page | `12d2-7651-633f-4d4b-a30d-e0e7cc9928b0` | Parent space for the agent family |
| Agents | page | `31f629c1-1208-8073-a296-c6ce5ee31826` | The agent catalog Hijack must slot into |
| Roadmap | page | `203629c1-1208-80b8-b60a-ea8f2e20055e` | Vision, goals, milestones |
| Knowledge Base | page | `1a6629c1-1208-81a2-b0e4-c510db54087c` | Intelligence library |
| Members | database | `206629c1-1208-807f-bbc6-fdee51b921e3` | Per-member pages (briefing dashboards land here) |
| Tasks | database | `25f23c51-16c2-4034-91c3-610209f1efc9` | Tasks; min fields: Name, Project, Status, Priority, Timeline |
| Deliverables | database | `2e3629c1-1208-81ba-98f1-c21ad0c15638` | Deliverables; roll up to Projects |
| SR Meetings | database | `299629c1-1208-8062-8230-cb0a55b157fd` | Meeting pages; data source `collection://299629c1-1208-8085-a950-000b26191eda` |
| Stakeholders | database | `1a6629c1-1208-8177-8918-c8c666f21abc` | CRM: actors, contacts, organisations |
| Systemic Investment Portfolio | page | `1a6629c1-1208-8145-a1d9-f9432fba488f` | Portfolio strategy + holdings |
| Weekly Team Debrief (template) | page | `325629c1-1208-8049-aac0-cafb4541fa03` | Format reference for weekly recaps |

> **To confirm before Hijack writes playbooks:** the exact deal-pipeline /
> "actors" data source. Search surfaced deal-like records (e.g. *Ardian x
> aDryada*, *2050*, *Mesa co-funding alliance*, *Tin Shed Ventures*) with
> properties such as `Actor Type`, `Capital Type`, `Currency`, `Date of
> Entry`, `Current Valuation`. These appear to live in the Stakeholders DB or a
> dedicated pipeline DB. Verify the data source URL and schema before wiring
> pipeline playbooks.

## Existing agent family (house style Hijack must match)

The `Agents` page already documents a consistent family. Names riff playfully
on team members.

| Agent | Owns |
| --- | --- |
| Arthuro | Project & task operations (transcripts → tasks, status summaries) |
| Aliché | Daily "Morning Sunshine" briefing + inbox/agenda pulse |
| Jilly Bean | Comms & outreach drafting + stakeholder enrichment |
| MarghaRita | Meeting prep, minutes, follow-ups |
| Lolo | Email assistant (Notion Mail) — draft-only |
| Pasqualhiño | Weekly recap across meetings, people, timelines |
| SeñoRita | Monday newsletter digest |

Each agent entry follows a fixed structure: **What it does / Use it when /
Typical outputs / Example prompts**, then a collapsible `### Instructions
{toggle="true"}` block holding the full operating spec. See
`docs/conventions/agent-spec-format.md` for the captured template.

## Culture and guardrails Hijack inherits

From **How we work**:

- Great work creates freedom; clear ownership, shared responsibility.
- Light structure, high clarity — "just enough" process.
- Meetings serve decisions and action; capture decisions, owners, next steps.
- Roadmap → Projects → Tasks. Projects need Name, Important, Urgent, Objective,
  Timeline, Lead. Tasks need Name, Project, Status, Priority, Timeline and must
  link to a Project.
- External meeting protocol: prep (context, CRM review, template) → during
  (decisions, open questions, next steps) → after (notes, CRM update, tasks).
- **Deep Work Day = Thursday** (no meetings by default).
- KYC / compliance check when entering new business relationships.

From **Good Practices in Using AI** (8 principles): critical AI literacy;
institutional guidelines; tool selection; setting limits; input-data
confidentiality & IP; reviewing outputs; transparency about AI use; ongoing
learning. The guide explicitly lists deal-making spaces AI can augment:
originating/analysing deals, conducting due diligence, locating funders,
ideating pitch decks, structuring vehicles, drafting term sheets.

These are the non-negotiable behavioural constraints baked into Hijack.
