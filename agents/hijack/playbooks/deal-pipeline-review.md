# Playbook: Deal Pipeline Review

> Hijack's read on the health of Arthur's Deal Flow pipeline: what moved, what's
> stale, and what's ready for the next gate.

## Trigger

- On demand: "Review my pipeline — what moved, what's stale, what's at the gate?"
- Optionally before pipeline/IC cadences.

## Source

- **Deal Flow** data source `collection://2a3629c1-1208-8081-babb-000b45feaf73`.
- Key properties: `Deal Stage`, `Owner`, `Sector`, `SIP`, `Capital Type`,
  `Currency`, `Date of Entry`, `Deadline`, `Next Board`, `Investment Date`.

## Stage model

Order deals along the real pipeline:

| Stage | Meaning |
| --- | --- |
| Just received | New inbound, not yet triaged. |
| To qualify | Screening against thesis/fit. |
| In DD / in DD - TGI | Due diligence underway (TGI = sub-track). |
| IC stage | At / heading to investment committee. |
| Executed - Direct portfolio | Closed, direct holding. |
| Executed - FoF portfolio | Closed, fund-of-funds holding. |
| Executed Philanthropy | Closed, philanthropic vehicle. |
| On hold / Parked | Paused. |
| Declined | Passed. |

## Output

A pipeline review written to the sandbox, dated:

```
# Pipeline Review — <YYYY-MM-DD>
> Drafted by Hijack — review before use.

### Snapshot
- Active deals by stage (count): a compact table or one line per stage.
- Movement since last review: deals that changed stage (from → to).

### Needs attention
- **Stale:** deals with no datable change in N days (default 21) for their
  stage — flag with days-in-stage.
- **At the gate:** deals in `IC stage` or late `In DD` — list what's still open
  before they can advance (DD completeness, KYC, red flags).
- **Dated this period:** Deadline / Next Board / Investment Date landing soon.

### Suggested next steps
- Per flagged deal: one concrete, proposed next action (a comment, not an edit).
```

## Staleness rule

- A deal is **stale** if it has no datable change (stage, dated milestone, or new
  document) within the staleness window for its stage. Defaults: To qualify 14d,
  In DD 21d, IC stage 7d. Make the window configurable.
- Report days-in-stage from `Date of Entry` or the last stage change you can
  evidence. If you can't evidence a date, say so — don't estimate.

## Rules

- Only Arthur's deals unless asked for the team view.
- Propose stage changes; never set them.
- Surface KYC/compliance as a gate item when a deal approaches `Executed`.
- Don't invent valuations or ticket sizes; pull only what's in the record.
