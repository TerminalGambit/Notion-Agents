# Hijack — Chief of Staff

**Hijack** is the Chief-of-Staff agent for **Arthur Garaud**, Co-Founder of
Systemic Regeneration. Where SR's other agents are functional helpers shared by
the whole team, Hijack serves one principal across his investment remit: deal
sourcing, due diligence, portfolio construction, and the regenerative-
entrepreneurship pipeline.

The name: a Chief of Staff *hijacks* the noise so the principal doesn't have to.
It also riffs on the SR habit of playful agent names (Arthuro, MarghaRita…).

## What Hijack is and isn't

- **Is:** a gatekeeper and synthesiser. It reads Arthur's surfaces, triages
  what matters, surfaces decisions, and drafts the artifacts — briefs, pipeline
  reviews, DD summaries, portfolio rollups, meeting prep.
- **Isn't:** a decision-maker or a task-doer. It doesn't run IC, make go/no-go
  calls, allocate capital, or duplicate Arthuro (tasks) / MarghaRita (minutes).
  It coordinates and lenses their output for Arthur.

## v1 scope and posture

- **Pure Notion agent.** No Gmail/Calendar/SDK yet — those come with later
  full-stack versions and other agents.
- **Draft-only / sandboxed.** Hijack reads and comments on production databases
  but only *writes* to its own sandbox page. The gate is structural: in Notion,
  grant Can edit on the sandbox and Can view/comment everywhere else.

## Files

| File | Purpose |
| --- | --- |
| `hijack.agent.md` | The spec — front matter + the Notion house block + Instructions. |
| `playbooks/weekly-exec-brief.md` | Monday brief for Arthur's week. |
| `playbooks/deal-pipeline-review.md` | Deal Flow health: movement, staleness, gates. |
| `playbooks/due-diligence-support.md` | DD checklist + scored rubric + impact gate. |
| `playbooks/portfolio-rollup.md` | SIP portfolio status and watch-list. |
| `playbooks/meeting-prep.md` | Pre-read + agenda for Arthur's external meetings. |
| `sandbox-test-plan.md` | Runbook for the first live sandbox test. |

## Status

Draft. Built via `workflows/build-and-optimize-agent.md`, grounded in
`docs/research/2026-05-29-cos-notion-agent-research.md`. The seed golden-task set
is in `evals/hijack/`.

**Next: sandbox test** (planned). Follow `sandbox-test-plan.md` — create the
sandbox page, set draft-only permissions, confirm the full Deal Flow schema,
dry-run each playbook on real data, grade against the golden tasks, and capture
failures before marking Hijack `active`.
