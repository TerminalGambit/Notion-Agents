# Notion-Agents

A version-controlled home for Systemic Regeneration's (SR) Notion agents:
their specifications, the research behind them, the conventions that keep them
consistent, and a workflow for building and optimising new ones.

The repo is the **source of truth**; Notion is the **deployment target**. We
author and review agent specs here as Markdown, then paste/sync them into the
Notion Agents space.

## What's here

| Path | Purpose |
| --- | --- |
| `agents/` | One folder per agent. Each holds a spec, a README, and playbooks. |
| `agents/_template/` | The canonical template for a new agent spec. |
| `agents/hijack/` | **Hijack** — the Chief-of-Staff agent for Arthur Garaud. |
| `docs/research/` | Cited research reports grounding design decisions. |
| `docs/architecture/` | How the agents fit together and the layouts they use. |
| `docs/conventions/` | Markdown style guide, agent-spec format, Notion conventions. |
| `docs/workspace/` | Map of the live SR workspace (IDs, databases, people, culture). |
| `workflows/` | The repeatable explore → design → test → optimise workflow. |
| `evals/` | Golden tasks and rubrics for measuring agent quality. |

## The agent family

Hijack joins an existing family already documented in the SR Notion workspace:
Arthuro (projects & tasks), Aliché (daily briefing), Jilly Bean (comms),
MarghaRita (meetings), Lolo (email), Pasqualhiño (weekly recap), SeñoRita
(newsletters). New agents must match that house style — see
`docs/conventions/agent-spec-format.md`.

**Hijack** is the Chief-of-Staff. Where the others are functional helpers,
Hijack serves one principal — Arthur — across his investment remit (deal
pipeline, due diligence, portfolio, weekly exec brief, meeting prep). v1 is a
**pure Notion agent** with a **draft-only / sandboxed** write posture. Later
versions go full-stack (Gmail, Calendar, Agent SDK, other MCP endpoints).

## Operating principles

These hold for every agent in this repo, inherited from SR's *How we work* and
*Good Practices in Using AI*:

- **Agents are helpers, not decision-makers.** Sanity-check before sharing.
- **Draft, don't apply.** Default to proposing changes; humans approve writes.
- **Never invent facts.** Leave fields empty and flag what's missing.
- **Be transparent** about AI involvement; respect confidentiality, IP, and KYC.
- **Clear ownership:** every output names decisions, owners, and next steps.
- Timezone is **Europe/Zurich**; **Thursday is Deep Work Day** (no meetings).

## Getting started

1. Read `docs/workspace/sr-workspace-map.md` for the lay of the land.
2. Read `docs/conventions/` before writing or editing any spec.
3. To build a new agent, follow `workflows/build-and-optimize-agent.md` and copy
   `agents/_template/AGENT_TEMPLATE.md`.
4. To understand Hijack, start at `agents/hijack/README.md`.
