# Research: Notion-native Chief-of-Staff Agents

> Cited research report grounding the design of **Hijack**, a Notion-native
> Chief-of-Staff agent for Arthur Garaud (Co-Founder, Systemic Regeneration).
> Compiled 2026-05-29 from six parallel research sweeps. Confidence levels and
> thin-evidence flags are preserved so we don't over-trust vendor numbers.

## How to read this

Each section lists key findings with sources and a confidence tag, then rolls
up into recommendations. The two consumers of these recommendations are:

- **(a) the repo foundation** — conventions, structure, evaluation workflow.
- **(b) the Hijack agent** — its architecture, scope, and guardrails.

A general caveat applies throughout: most vendor performance and adoption
numbers (token savings, time saved, IRR, adoption %) were reachable only via
search snippets, not fetched primary pages. Treat them as directional. The
architectural patterns and documented product behaviours are high-confidence.

## 1. Chief-of-Staff agent architectures

- **Start simple; escalate only when a single loop fails.** The
  augmented-LLM (model + retrieval + tools + memory) is the atomic unit; add
  agentic complexity only when warranted. *(High —
  [Anthropic: Building effective agents](https://www.anthropic.com/research/building-effective-agents))*
- **Workflows vs agents.** Anthropic separates *workflows* (predefined code
  paths) from *agents* (model directs its own process), and names five
  composable patterns: prompt chaining, routing, parallelization,
  orchestrator-workers, evaluator-optimizer. Recurring CoS tasks (the weekly
  brief) map to deterministic workflows; open-ended asks map to
  orchestrator-workers. *(High — same source)*
- **The human CoS role = three functions:** strategic alignment/prioritization,
  operational execution, and communication/filtering (gatekeeping). The
  gatekeeping/filtering function — deciding what reaches the principal — is the
  core value, serving one principal. *(High —
  [chiefofstaff.network](https://www.chiefofstaff.network/blog/what-does-a-chief-of-staff-do-a-guide-to-the-most-versatile-role-in-business))*
- **Memory splits three ways:** episodic (what happened), semantic (durable
  facts/preferences about the principal), procedural (skills/workflows, usually
  in the prompt). Notion databases are a natural backing store for episodic +
  semantic. *(High — consensus across IBM, LangChain, Mem0, Letta;
  [Atlan](https://atlan.com/know/types-of-ai-agent-memory/))*
- **The "daily/weekly briefing" is the flagship recurring pattern.** Instead of
  the principal polling each tool, the agent polls all sources on a schedule,
  synthesizes, prioritizes, and delivers a short digest at a fixed time.
  Microsoft (Copilot Chief-of-Staff persona) and Google (Gemini Daily Brief)
  both ship versions. *(Med — pattern well-attested; specific stats are
  single-source marketing)*
- **Human-in-the-loop = interrupt/resume.** Gate sensitive/irreversible actions
  (send email, create/delete/share) behind an approval interrupt; let
  read/synthesize steps run autonomously. Two oversight modes: synchronous
  approval vs asynchronous audit. *(High —
  [OpenAI Agents SDK HITL](https://openai.github.io/openai-agents-python/human_in_the_loop/),
  [LangGraph interrupts](https://blog.langchain.com/making-it-easier-to-build-human-in-the-loop-agents-with-interrupt/))*
- **Single-principal personalization compounds.** The premium-EA model (Athena)
  works exclusively for one client and grows more valuable as it accumulates
  context — validating a one-principal design with growing semantic memory.
  *(Med — partly marketing over a human service)*

## 2. Notion-native agents and the API/MCP

- **Two products:** the interactive *personal Notion Agent* and autonomous
  *Custom Agents* (schedule/trigger-driven background agents). A CoS maps to a
  Custom Agent. Custom Agents (3.3, Feb 2026) run on schedules
  (daily/weekly/monthly) or events (comment added, page added to DB, property
  updated, page removed). *(High —
  [Custom Agents help](https://www.notion.com/help/custom-agents),
  [3.3 release](https://www.notion.com/releases/2026-02-24))*
- **Build-from-nothing security.** Custom Agents start with zero access and
  inherit nothing; they only touch explicitly granted resources. Per-resource
  levels are **Can view / Can comment / Can edit**. Workspace-wide access is off
  by default behind a warning modal. *(High —
  [How we built security into Custom Agents](https://www.notion.com/blog/how-we-built-security-into-custom-agents),
  [sharing & permissions](https://www.notion.com/help/custom-agents-sharing-and-permissions))*
- **Auditable and reversible.** Every run is logged; changes are reversible via
  version history. *(High —
  [security features](https://www.notion.com/help/custom-agents-security-features))*
- **No delete tool.** The MCP surface (`notion-fetch`, `-search`,
  `-query-data-sources`, `-create-pages`, `-update-page`,
  `-update-data-source`, `-create-comment`, …) exposes **no delete** for pages
  or databases. Worst-case blast radius is a reversible edit, not data loss.
  *(High — confirmed against the live toolset)*
- **Markdown-first MCP.** The hosted MCP uses "Notion-flavored Markdown" rather
  than raw JSON, for token efficiency. *(High —
  [hosted MCP](https://www.notion.com/blog/notions-hosted-mcp-server-an-inside-look))*
- **Databases vs data sources (API 2025-09-03).** A database is a container; the
  table/schema is a *data source* with its own `data_source_id`. Resolve
  database → data source before querying. *(High —
  [upgrade guide](https://developers.notion.com/docs/upgrade-guide-2025-09-03))*
- **Limits (verify before hardcoding):** ~100 results/page, ~10k/query ceiling,
  ~3 req/s (429 + `Retry-After`), payloads ~1000 blocks / ~500 KB, rich text
  ~2000 chars, ~2-level nesting. *(Med/Low — snippet-derived; we already hit a
  429 during recon, so backoff is real)*

## 3. Agent instruction / spec conventions

- **Two-layer model.** Use a repo-level, tool-agnostic
  [AGENTS.md](https://agents.md/) ("README for agents") for shared context, and
  per-agent spec files for task-specialized agents. AGENTS.md is the cross-vendor
  standard (Codex, Cursor, Jules, Amp; stewarded under the Linux Foundation) and
  composes nearest-file-wins. *(High)*
- **Claude Code subagents** are Markdown + YAML frontmatter; `name` +
  `description` required, body is the system prompt verbatim; `description` is
  the routing signal. Best practice: one focused task per agent, least-privilege
  tools, version-controlled. *(High —
  [sub-agents docs](https://code.claude.com/docs/en/sub-agents))*
- **Prompt structure** (Anthropic): role → clear explicit task → motivation/
  context → 3–5 diverse `<example>`-tagged examples → output format; use XML
  tags to separate sections. Newer models (Opus 4.x) follow instructions more
  literally — state scope explicitly and strip stale scaffolding. *(High —
  [prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices))*
- **Right altitude.** Avoid both brittle hardcoded logic and vague hand-waving;
  curate a few canonical examples over exhaustive edge cases. *(Med —
  [context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents), 403'd)*
- **Factor out shared snippets** rather than copy-pasting; keep always-on
  context lean. *(Med)*

## 4. Markdown clarity and consistency

- **Structure helps comprehension and saves tokens.** Markdown is model-parsable
  and cheaper than HTML; structure aids recall — though the headline numbers
  (80% token savings, 30% recall lift) are vendor/blog claims, not benchmarks.
  Markdown awareness *varies by model*, so keep structure simple and explicit.
  *(Mixed — [MDEval, ACM Web Conf 2025](https://arxiv.org/abs/2501.15000) is
  the solid anchor)*
- **Enforceable structure rules** (markdownlint): one H1 (MD025), increment
  headings by one (MD001), consistent list markers/numbering (MD004/MD029),
  blank lines around blocks (MD022/MD032), table consistency (MD055/MD056).
  Reconcile with Prettier via the `markdownlint/style/prettier` preset. *(High —
  [markdownlint Rules](https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md))*
- **Prose linting** (Vale) catches what markdownlint can't: terminology, passive
  voice, banned filler. Datadog runs markdownlint + Vale in CI across 20k+
  PRs/yr. *(High —
  [Datadog/Vale](https://www.datadoghq.com/blog/engineering/how-we-use-vale-to-improve-our-documentation-editing-process/))*
- **Notion is lossy.** Headings deeper than H3 collapse to H3; inline images get
  extracted; long content is auto-split. **Cap at H3** and avoid inline images
  for anything that syncs to Notion. *(High —
  [martian](https://github.com/tryfabric/martian))*
- **Notion-flavored markdown** extends CommonMark with XML-like tags
  (`<callout>`, toggles via `{toggle="true"}`, `<columns>`, `<mention-page>`)
  and tab-based child nesting. Document it separately from standard markdown.
  *(Med — official page 403'd; corroborated)*
- **One topic per file; headings as chunk boundaries** for clean retrieval.
  Adopt Google's developer style guide as a base + a short repo addendum. *(Med)*

## 5. Venture / PE workflow patterns

- **Standard pipeline = ~6 stage gates:** sourcing → screening → partner review
  → due diligence → IC → close/deploy, then portfolio monitoring; model as a
  single-select stage with entered-dates for deterministic analytics. *(High —
  [Affinity](https://www.affinity.co/blog/structured-deal-pipelines-vc-dealmaking))*
  **SR's actual `Deal Flow` stages:** Just received → To qualify → In DD /
  in DD - TGI → IC stage → Executed (Direct portfolio / FoF portfolio /
  Philanthropy), plus On hold / Parked / Declined.
- **DD checklist = nine areas:** finance, tax, legal, HR, assets, IT,
  products/services, marketing & sales, founder background; collapse evaluation
  to four scored judgments: team, market, financial, legal (team weighted
  heavily). Maintain a red-flags multi-select. *(High —
  [Affinity DD checklist](https://www.affinity.co/guides/due-diligence-checklist-for-venture-capital))*
- **AI cuts manual DD effort** (document parsing, checklist population,
  benchmarking) but the go/no-go stays human; sourcing tools (Harmonic, Grata,
  PitchBook, Dealroom) are upstream feeds, not things to reinvent. *(Med on the
  "70%" figure;
  [Harmonic](https://harmonic.ai/blog/how-harmonic-serves-venture-capital-firms))*
- **Power-law portfolio construction** + explicit dials (initial vs reserve
  split, ownership targets). *(High —
  [The VC Factory](https://thevcfactory.com/vc-portfolio-construction/))*
- **Venture studios differ:** co-found from day one, take larger equity; the
  "pipeline" is idea → validation → build → spin-out. Keep both fund-style and
  studio-style templates. *(Med — GSSN outcome figures are self-selected)*
- **Impact / regenerative assessment** standard is the Impact Management Project
  **Five Dimensions** (What, Who, How Much, Contribution, Risk) + the three
  tests (intentionality, measurability, additionality). For SR's regenerative
  mandate, run impact diligence as a gate equal to financial diligence. *(High —
  [Sopact](https://www.sopact.com/use-case/five-dimensions-of-impact))*

## 6. Evaluation and optimization

- **Error analysis first, then golden tasks.** Open-code ~30–50 real traces by
  hand, cluster, review ≥100; build metrics from *observed* failure modes, not
  generic "helpfulness." *(High —
  [Hamel: evals FAQ](https://hamel.dev/blog/posts/evals-faq/))*
- **Per-axis rubrics, isolated judges, calibrated to humans.** Score separate
  axes (factual accuracy, completeness/prioritization, actionability,
  tone/format), one judge call per axis; a single call returning a 0–1 score +
  pass/fail is the most human-aligned setup. *(High —
  [Anthropic: demystifying evals](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents))*
- **Binary pass/fail beats Likert** for consistency. *(High — Hamel)*
- **Three grader types:** code-based (structural facts: right meeting, date,
  owner, link), model-based (subjective prose quality, think-then-discard), and
  human (gold standard). Prefer volume of auto-gradeable cases. *(High —
  [Anthropic develop-tests](https://docs.anthropic.com/en/docs/test-and-evaluate/develop-tests))*
- **Validate the judge** against human labels (Cohen's Kappa) before trusting
  it; re-check after prompt changes. Mitigate position/verbosity bias by
  swapping order and balancing length. *(High — Hamel;
  [OpenAI evals](https://developers.openai.com/cookbook/examples/evaluation/getting_started_with_openai_evals))*
- **Criteria drift is real** — rubrics get refined only after seeing outputs;
  iterate the rubric, don't freeze it. *(High —
  [EvalGen, arXiv:2404.12272](https://arxiv.org/abs/2404.12272))*
- **Report pass rates with confidence intervals** (Wilson) and treat prompt
  changes as experiments; a 2/30 swing is noise. Tooling: a code-first harness
  (Promptfoo / DeepEval) in-repo for CI + a platform (Braintrust / LangSmith /
  Phoenix) only when you need annotation history. *(High method / Med tooling)*

## Recommendations

### (a) Repo foundation

1. **Two-layer specs.** Root-level shared context (this repo's README + a
   markdown style guide + Notion conventions) plus one self-contained spec file
   per agent following a fixed template (see
   `docs/conventions/agent-spec-format.md`). One responsibility per agent;
   least-privilege access stated explicitly.
2. **Match the existing SR house style exactly** (What it does / Use it when /
   Typical outputs / Example prompts + a collapsible Instructions block). The
   repo is the version-controlled source of truth; Notion is the deployment
   target.
3. **Markdown discipline:** one H1, increment headings, ≤H3 (Notion-safe), one
   topic per file, small tables, ordered lists for steps. Ship a
   `.markdownlint.json` and (optionally) Vale config so consistency is
   enforceable, not aspirational.
4. **Lightweight eval workflow:** golden tasks built from real failure modes,
   per-axis binary rubric, a calibrated judge, pass rates with CIs, run on every
   spec change. Start in-repo (no heavy platform).
5. **A documented build-and-optimize workflow** (`workflows/`) so every future
   agent goes through the same explore → design → test → optimize loop.

### (b) Hijack design

1. **Gatekeeper first, doer second.** Primary loop: ingest (Deal Flow, Tasks,
   Deliverables, Meetings, Stakeholders) → triage/prioritize for Arthur →
   surface what needs his attention → draft. Generic task execution is
   secondary and already covered by Arthuro.
2. **Notion-only v1, draft-only posture enforced structurally.** Read/comment on
   production DBs; write only to a dedicated **Hijack** sandbox page. Rely on
   build-from-nothing permissions, the absence of a delete tool, and version
   history as the safety net. Turn web access off unless a playbook needs it.
3. **Weekly Exec Brief is the flagship deliverable**, modeled on the existing
   Weekly Team Debrief format but lensed for Arthur's investment remit.
4. **Model the investment work on SR's real structures:** the `Deal Flow`
   stages above; a DD support playbook using the nine-area checklist + four
   scored judgments + red flags + the IMP Five Dimensions as a parallel,
   equal-weight impact gate; a portfolio rollup over the SIP buckets. Hijack
   *populates and proposes*; humans decide (IC, go/no-go, capital).
5. **Respect SR culture & AI principles:** clear ownership, decisions/owners/
   next steps, KYC/compliance reminders, transparency about AI use, never
   invent facts, leave fields empty when unsure. Europe/Zurich; respect Deep
   Work Thursdays.
6. **Build for limits:** resolve data-source IDs, paginate, back off on 429,
   chunk long drafts. Keep numeric limits configurable since they're unverified.

## Open questions / thin evidence

- Exact Notion API numeric limits — verify against live docs before relying on
  them.
- Whether Custom Agents get a personal-Agent-style pre-execution "Plan mode"
  approval gate — unconfirmed; we enforce the gate via permissions instead.
- Vendor performance numbers (DD time saved, studio IRR, adoption %) are
  directional, not audited.
- The `Deal Flow` schema beyond the stage list (full property set, IC/portfolio
  sub-structure) should be confirmed live before automating writes.
