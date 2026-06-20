# Agent Spec Format

> The canonical structure for every agent in this repo. It marries SR's existing
> Notion house style (so specs paste cleanly into the Agents page) with
> version-control conventions from the research report (§3).

## Two layers

Each agent has **two representations of the same spec**:

1. **Repo spec** (`agents/<name>/<name>.agent.md`) — the source of truth.
   YAML front matter + a Markdown body. Reviewed like code.
2. **Notion entry** — the body, pasted into the SR *Agents* page, following the
   house block below. The collapsible Instructions block in Notion is the literal
   operating prompt.

Keep them in sync: edit the repo spec, then update Notion.

## Front matter (repo spec)

```yaml
---
name: hijack                      # globally unique, lowercase-hyphen
tagline: Chief of Staff           # short role label
principal: Arthur Garaud          # who it serves (CoS agents serve one person)
status: draft                     # draft | active | retired
write_posture: draft-only         # read-only | draft-only | full-write
surfaces:                         # Notion DBs/pages it reads or writes
  reads: [Deal Flow, Tasks, Deliverables, SR Meetings, Stakeholders]
  writes: [Hijack sandbox page]
timezone: Europe/Zurich
owner: arthur.garaud@edr-sr.com
updated: 2026-05-29
---
```

`name` and `tagline` are required; the rest are strongly recommended. `surfaces`
and `write_posture` make the least-privilege scope explicit and reviewable.

## Body structure (the house block)

This is the exact SR convention, observed across Arthuro, Aliché, MarghaRita,
Lolo, Pasqualhiño. Match it.

```markdown
### {Name} — {Tagline}

**What it does**
- One-line capability.
- One-line capability.

**Use it when**
- Trigger situation.

**Typical outputs**
- Concrete artifact it produces.

**Example prompts**
- "A realistic thing the principal would type."

### Instructions {toggle="true"}
	## 📖 Overview
	...the literal operating prompt...
	## ✅ When to run
	## 🔎 What to extract / inputs
	## 🧾 Output
	## 🧭 Linking & data rules
	## 🚫 If unsure
	## Guardrails
```

Notes on the Instructions block:

- It is **collapsible** (`{toggle="true"}`) and its children are **tab-indented**
  one level — that's Notion-flavored markdown for nested blocks.
- Use **emoji-prefixed section headings** (📖 ✅ 🔎 🧾 🧭 🚫) to match the family.
- Reference Notion databases with `<mention-page url="...">` in the Notion
  version; in the repo spec, name them and link the workspace map.

## Required sections in Instructions

Every agent's Instructions must cover:

- **Overview** — one paragraph: what it does and the source/target databases.
- **When to run** — triggers (schedule, event, or on-demand mention).
- **Inputs / what to extract** — what it reads and what counts as signal.
- **Output** — exact format and where it's written.
- **Linking & data rules** — how it maps to owners, projects, deliverables,
  stages; what it must not invent.
- **If unsure** — the fallback (leave empty, add a note, ask). Never guess.
- **Guardrails** — write posture, confidentiality, KYC, "don't change schemas",
  loop-avoidance, transparency.

## Conventions every agent inherits

From the SR *Agents* page "Notes / team conventions":

- Agents are **helpers, not final decision-makers**. Sanity-check outputs.
- Every ask to an agent should include: **the relevant page/DB link**, **the
  target audience**, and **the desired output format**.
- **Never invent** owners, dates, contacts, or facts. Prefer correctness over
  completeness; if a required input is missing, say so and stop.

## Playbooks

Complex agents split repeatable jobs into **playbooks** under
`agents/<name>/playbooks/`. A playbook is a focused procedure (e.g. "weekly exec
brief") with its own trigger, steps, and output contract. The main spec lists
and links them; the playbook file holds the detail. This keeps the spec at the
"right altitude" and the procedures composable.
