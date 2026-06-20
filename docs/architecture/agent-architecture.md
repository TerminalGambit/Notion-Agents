# Agent Architecture

> How SR's agents fit together, and the architecture Hijack uses. Grounded in
> the research report (§1, §2).

## The family, layered

SR's agents fall into two layers:

- **Functional helpers** (existing): each owns one job across the whole team —
  Arthuro (tasks), MarghaRita (meetings), Lolo (email), Jilly Bean (comms),
  Pasqualhiño (weekly recap), Aliché (daily briefing), SeñoRita (newsletters).
- **Principal agents** (new, starting with Hijack): each serves **one person**
  across their remit, orchestrating and synthesising — not duplicating — the
  functional helpers.

```
                 ┌─────────────────────────────┐
   Arthur  ◀────▶│   Hijack (Chief of Staff)   │
                 │  gatekeeper · synthesiser    │
                 └──────────────┬──────────────┘
                                │ reads / coordinates
   ┌──────────┬──────────┬──────┴─────┬──────────┬───────────┐
 Arthuro   MarghaRita   Lolo      Jilly Bean  Pasqualhiño   Aliché
 (tasks)   (meetings)  (email)    (comms)     (wk recap)  (daily brief)
                                │ all read/write
                 ┌──────────────┴──────────────┐
                 │   Notion workspace (truth)   │
                 │ Deal Flow · Tasks · Meetings │
                 │ Deliverables · Stakeholders  │
                 └─────────────────────────────┘
```

Hijack doesn't re-implement task extraction or meeting minutes; it reads the
same databases those agents maintain and lenses them for Arthur.

## Hijack's internal architecture

Per the research, **start with a single augmented LLM**, not a multi-agent
system. Hijack is one agent with:

- **Tools:** the Notion MCP surface (fetch, search, query-data-sources,
  create-comment; create/update only on the sandbox).
- **Memory, backed by Notion pages** (not a separate store):
  - *Semantic* — Arthur's priorities, preferences, and standing context, kept on
    a Hijack "instructions / context" sub-page he can edit.
  - *Episodic* — a dated log of briefs produced and decisions surfaced, in the
    sandbox.
  - *Procedural* — the playbooks in this repo (and the Notion Instructions
    block).
- **A router** that picks the mode per request:
  - **Workflow mode** (deterministic) for recurring deliverables — the Weekly
    Exec Brief, pipeline review, portfolio rollup. These follow a fixed
    procedure.
  - **Agent mode** (dynamic) for open-ended asks — "help me prep for the
    Systemiq IC", "what's stale in my pipeline and why".

Reserve multi-agent fan-out (orchestrator-workers) for genuinely broad synthesis
tasks; it costs roughly an order of magnitude more tokens and isn't needed for
routine ops.

## The gatekeeper loop

Hijack's primary loop mirrors the human CoS's core function — filtering, not
doing:

1. **Ingest** from Arthur's surfaces (Deal Flow, Tasks/Deliverables he owns or
   leads, SR Meetings he participates in, relevant Stakeholders).
2. **Triage & prioritise** — what changed, what's at risk, what's blocking, what
   needs a decision.
3. **Surface** — present the short list that deserves Arthur's attention, with
   context and a recommended next step.
4. **Draft** — produce the artifact (brief, agenda, DD summary, follow-up) into
   the sandbox or as comments, for Arthur to approve.

## Human-in-the-loop

Actions are tiered:

- **Autonomous (read/synthesise):** querying, summarising, drafting into the
  sandbox, commenting. Logged, reversible, low blast radius.
- **Gated (would change shared state):** editing production pages, changing
  properties/stages, anything outbound. Hijack **proposes**; Arthur approves and
  applies. In v1 this gate is structural — Hijack simply lacks edit permission
  outside the sandbox.

This keeps Hijack safe by construction while the trust model matures. Full-stack
versions (Gmail/Calendar/SDK) will add an explicit interrupt/resume approval gate
for outbound actions.

## Why Notion-only for v1

- The safety model (build-from-nothing permissions, no delete tool, version
  history) makes a draft-only agent safe with little custom engineering.
- Arthur's investment work already lives in Notion (Deal Flow, SIP, Meetings),
  so a Notion-native agent reaches 80% of the value before any external
  integration.
- It de-risks the trust relationship before Hijack touches email or calendar.
