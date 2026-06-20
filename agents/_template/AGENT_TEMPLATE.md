---
name: agent-name                  # globally unique, lowercase-hyphen
tagline: Short role label
principal: Team                   # a person (for CoS agents) or "Team"
status: draft                     # draft | active | retired
write_posture: draft-only         # read-only | draft-only | full-write
surfaces:
  reads: [Database A, Database B]
  writes: [Sandbox page]
timezone: Europe/Zurich
owner: someone@edr-sr.com
updated: YYYY-MM-DD
---

# {Name}

> One-paragraph description of what this agent is and who it serves. Link to its
> playbooks and to `docs/conventions/agent-spec-format.md`.

## Notion entry

Paste the block below into the SR *Agents* page. Keep it in sync with this file.

### {Name} — {Tagline}

**What it does**
- Capability in one line.
- Capability in one line.

**Use it when**
- Trigger situation.

**Typical outputs**
- Concrete artifact.

**Example prompts**
- "Realistic ask the principal would type."

### Instructions {toggle="true"}
	## 📖 Overview
	What it does and the source/target databases (mention them).

	## ✅ When to run
	Schedule, event trigger, or on-demand mention.

	## 🔎 Inputs / what to extract
	What it reads and what counts as signal vs noise.

	## 🧾 Output
	Exact format and where it's written.

	## 🧭 Linking & data rules
	How it maps owners, projects, deliverables, stages. What it must not invent.

	## 🚫 If unsure
	The fallback: leave empty, add a note, or ask. Never guess.

	## Guardrails
	Write posture, confidentiality, KYC, "don't change schemas",
	loop-avoidance, transparency about AI authorship.

## Playbooks

- `playbooks/example.md` — one-line description.
