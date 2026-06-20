# Workflow: Build and Optimise an Agent

> The repeatable loop for taking a new agent from idea to a tested, consistent,
> deployed spec. Every agent in this repo goes through it. This is the workflow
> the user asked for: explore resources → design → test → optimise.

Five phases. Don't skip the explore phase — most agent failures come from not
understanding the workspace, not from weak prompts.

## Phase 1 — Explore resources

Goal: understand the terrain before designing anything.

1. **Map the workspace.** Read `docs/workspace/sr-workspace-map.md`. Confirm the
   databases, pages, people, and IDs the agent will touch are still accurate
   (re-fetch live if in doubt). Add anything new to the map.
2. **Read the relevant production data, read-only.** Fetch a few real records
   the agent will operate on (e.g. a Deal Flow page, a meeting, a member page).
   Learn the real schema: property names, select options, page-body structure.
   *Discovery beats assumption — Hijack's stage list came from one real deal
   page, not a guess.*
3. **Study the house style.** Read the existing SR *Agents* page and the closest
   existing agent. Match its structure and tone.
4. **Research externally if the domain is unfamiliar.** Use the deep-research
   workflow (fan-out searches, fetch primary sources, verify claims, cite) and
   save the report to `docs/research/`. Flag thin evidence; don't hardcode
   vendor numbers.

Output of this phase: an updated workspace map and (if needed) a cited research
report.

## Phase 2 — Design

Goal: a reviewable spec at the right altitude.

1. **Scope to one responsibility** (or one principal, for CoS-style agents).
   Don't duplicate what another agent already owns.
2. **Choose the write posture** — read-only, draft-only, or full-write — and
   make it least-privilege. Default to draft-only.
3. **Copy `agents/_template/AGENT_TEMPLATE.md`** and fill it: front matter, the
   house block (What it does / Use it when / Typical outputs / Example prompts),
   and the Instructions sections.
4. **Split repeatable jobs into playbooks** under `playbooks/`. Keep the main
   spec lean and link out.
5. **Write 3–5 canonical examples**, not exhaustive edge cases. Be explicit
   about scope (newer models follow instructions literally).
6. **Self-check against the conventions** (`docs/conventions/`) and run
   markdownlint.

Output: a draft spec + playbooks, lint-clean.

## Phase 3 — Test (build the eval set)

Goal: know what "good" means before optimising. Error-analysis-first.

1. **Generate real outputs.** Run the agent (or simulate it) on real workspace
   data to produce ~20+ outputs.
2. **Open-code failures by hand** on 30–50 of them: note what actually goes
   wrong (missed a blocking item, wrong owner, invented a fact, wrong format).
3. **Cluster the notes** into recurring failure modes.
4. **Turn the top failure modes into golden tasks** with binary, per-axis
   pass/fail criteria (factual accuracy, completeness/prioritisation,
   actionability, tone/format). Save to `evals/<name>/`.
5. **Add code checks** for verifiable facts (right meeting/date/owner/link) and
   reserve an LLM judge for subjective prose quality.
6. **Calibrate the judge** against your own labels (aim for high agreement)
   before trusting it.

Output: a golden-task set + rubric in `evals/<name>/`.

## Phase 4 — Optimise

Goal: improve the spec without fooling yourself.

1. **Treat each prompt change as an experiment.** Run the full golden set before
   and after.
2. **Report pass rates with confidence intervals** (Wilson). A 2/30 swing is
   noise — don't ship on it.
3. **Iterate the rubric too** (criteria drift): when grading reveals a new
   quality dimension, add it. When a new failure appears, add a regression case.
4. **Mind cost/latency.** A statistically real gain that doubles latency may not
   be worth it.

Output: a measurably better spec + an updated eval set.

## Phase 5 — Deploy and maintain

1. **Sync to Notion.** Paste the body into the *Agents* page following the house
   block; the Instructions toggle becomes the live operating prompt. Set the
   real per-resource permissions to match the declared write posture.
2. **Announce** it on the Agents page overview list.
3. **Commit and PR** the repo spec (draft PR by default).
4. **Re-validate on model upgrades.** Newer models follow instructions more
   literally — re-run the golden set and strip stale scaffolding when the model
   changes.
5. **Periodically re-cluster** fresh production traces so the eval set tracks
   real usage.

## The loop, in one line

Explore the real workspace → design a least-privilege spec in the house style →
build evals from real failures → optimise against them with CIs → deploy
draft-first and keep the eval set alive.
