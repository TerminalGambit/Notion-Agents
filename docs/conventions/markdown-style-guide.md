# Markdown Style Guide

> How we write Markdown in this repo so it's clear to humans, parses cleanly for
> LLM agents, and survives the round-trip into Notion. Grounded in the research
> report (§4) and the lossy-conversion limits of Notion.

## Why this exists

Document structure measurably affects how well models read and how cleanly text
chunks for retrieval. Consistency also lets us lint mechanically instead of
arguing in review. The rules below are the few that carry the most weight.

## Core rules

1. **One H1 per file**, used as the title. Everything else is H2/H3.
   *(markdownlint MD025)*
2. **Increment headings by one** — never jump H1 → H3. *(MD001)*
3. **Cap heading depth at H3.** Notion collapses anything deeper to H3, so
   `####` would silently flatten on sync. Use bold lead-ins or lists instead.
4. **One topic per file.** Section headings are natural chunk boundaries; keep
   each file scoped to a single subject so retrieval stays coherent.
5. **Blank lines around headings, lists, and code blocks.** *(MD022/MD032)*
6. **Consistent list markers:** `-` for unordered, `1.` for ordered. Use ordered
   lists only when sequence matters (steps); bullets for unordered sets.
   *(MD004/MD029)*
7. **Small tables only.** Tables are for compact comparison grids. If a table
   grows wide or deep, switch to subsections. Keep column counts equal.
   *(MD055/MD056)*
8. **Wrap prose at ~88 characters.** Keeps diffs readable. Don't hard-wrap
   inside tables or links.
9. **Sentence-case headings.** "Deal pipeline review", not "Deal Pipeline
   Review".
10. **Descriptive link text** — never "click here". Use absolute URLs for
    external sources, repo-relative paths for internal links.

## Prose

- **Active voice, present tense, second person** for instructions ("Read the
  deal page", not "The deal page should be read").
- **Avoid filler** — "simply", "just", "easily", "obviously". If it's easy, the
  reader will notice; if it isn't, the word is a lie.
- **Define a term once**, then use it consistently. Agent names are proper nouns
  (Hijack, Arthuro). Database names match Notion exactly (Deal Flow, SR
  Meetings, Stakeholders).
- **No em dashes in agent-authored drafts** that go to people — SR's existing
  email agent (Lolo) bans them as AI-tells. In repo docs they're fine.

## File naming

- `kebab-case.md` for docs. Research reports are prefixed with an ISO date:
  `YYYY-MM-DD-topic.md`.
- Agent spec files: `<name>.agent.md`. Playbooks: `kebab-case.md` under the
  agent's `playbooks/`.

## Front matter

Agent spec files carry a small YAML front-matter block (see
`agent-spec-format.md`). Other docs don't need front matter; a single H1 title
and a `>` blockquote summary at the top is enough.

## Notion safety checklist

Before a doc is meant to sync into Notion, confirm:

- [ ] No headings deeper than H3.
- [ ] No inline images (Notion extracts them into standalone blocks).
- [ ] No single block likely to exceed Notion's size limits — chunk long content.
- [ ] Notion-only syntax (callouts, toggles, columns, mentions) is written per
      `notion-conventions.md`, not invented.

## Enforcement

Structure and prose are meant to be machine-checked, not eyeballed:

- **markdownlint** for structure. A starter `.markdownlint.json` lives at the
  repo root. Reconcile with Prettier using the `markdownlint/style/prettier`
  preset so the two don't fight.
- **Vale** (optional, recommended) for prose: active voice, banned filler, and a
  shared term list (agent and database names).

Run them locally before opening a PR; wire them into CI when the repo gets a CI
config.
