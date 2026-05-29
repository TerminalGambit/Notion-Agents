# Playbook: Portfolio Rollup

> Hijack's periodic status read on the executed portfolio, organised by SIP
> (Systemic Investment Portfolio) bucket, with a watch-list of holdings that
> need attention.

## Trigger

- On demand: "Roll up the portfolio status."
- Optionally monthly, or before a board/committee deck.

## Source

- **Deal Flow** records in `Executed - Direct portfolio`,
  `Executed - FoF portfolio`, and `Executed Philanthropy`.
- The **Systemic Investment Portfolio** page for strategy/bucket context.
- Properties: `SIP` bucket, `Capital Type`, `Currency`, `Investment Date`,
  `Current Valuation` (if present), `Next Board`, `Owner`, `Sector`.

## Output

A portfolio rollup written to the sandbox, dated:

```
# Portfolio Rollup — <YYYY-MM-DD>
> Drafted by Hijack — review before use.

### By SIP bucket
For each SIP bucket: count of holdings, capital type mix, sectors, and any
movement since last rollup. Small table, one row per holding where useful.

### Watch-list
Holdings needing attention, each with the reason:
- No update / stale (no datable change in N days).
- Upcoming Next Board or reporting date.
- Known risk flag carried over from DD.

### Coverage gaps
- Holdings missing key data (valuation, last update, owner) — list, don't guess.
```

## Rules

- **Report only what's recorded.** Don't compute returns/multiples unless the
  underlying numbers are present and current; if they're not, say "not recorded".
- Group by SIP bucket; within a bucket, sort by attention needed.
- Flag stale holdings (default: no datable update in 90 days) and upcoming board
  dates.
- Capital allocation, reserves, and follow-on decisions are **human calls** —
  surface pacing/coverage facts, don't recommend deployment.
- Note KYC/compliance items still open on any holding.

## Quality bar

- Does the rollup show, per bucket, what's healthy and what needs a look — with
  every figure traceable to the record and nothing invented?
