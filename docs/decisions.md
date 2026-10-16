# Decisions

The log of every product and technical decision. Rules for statuses and
changes are in [`AGENTS.md`](../AGENTS.md#decision-statuses). Newest at the bottom.

Entry format:

```
## D-NNN: Title
- Status: Idea | Proposed | Agreed | Rejected | Superseded by D-NNN
- Type: Product | Technical
- Date: YYYY-MM-DD
- Decided by: Mark | Claude | GPT (proposer if not yet decided)
- Supersedes: D-NNN (if any)

What was decided, why, and the consequences. Alternatives considered.
```

---

## D-001: Project files are the shared source of truth

- Status: Proposed
- Type: Technical
- Date: 2026-10-16
- Decided by: Claude (proposer)

Markdown files in this repository are the only shared memory between GPT and
Claude Code. Chat history is for exploration. Neither assistant can read the
other's conversations, so anything that matters is written down.

## D-002: Division of responsibilities

- Status: Proposed
- Type: Product
- Date: 2026-10-16
- Decided by: Claude (proposer)

Mark is product owner. GPT owns product requirements, product decisions and
the roadmap. Claude Code owns implementation, tests and technical records.
Either may propose changes anywhere; changes to agreed requirements need
Mark's approval. Details and file ownership in `AGENTS.md`.

## D-003: Minimal documentation structure, grown on demand

- Status: Proposed
- Type: Technical
- Date: 2026-10-16
- Decided by: Claude (proposer)

Start with `AGENTS.md`, `CLAUDE.md`, `docs/workflow.md`, `docs/decisions.md`,
`docs/questions.md` and `docs/research/`. Create `product.md`, `roadmap.md`,
`plans/`, `technical.md` and `progress.md` when they first have content. One
decision log doubles as the ADR record rather than one file per ADR, until it
gets unwieldy.

Alternative considered: a full skeleton up front. Rejected because empty files
go stale and make it unclear what's real.

## D-004: Shared instructions in AGENTS.md, Claude imports them

- Status: Proposed
- Type: Technical
- Date: 2026-10-16
- Decided by: Claude (proposer)

Codex reads `AGENTS.md` natively. Claude Code reads `CLAUDE.md`, which imports
`AGENTS.md` with `@AGENTS.md` and adds Claude-only rules. One copy of the shared
rules, no duplication.

## D-005: GPT ↔ Claude communication approach

- Status: Proposed
- Type: Technical
- Date: 2026-10-16
- Decided by: Claude (proposer)

Use the shared-document workflow now, with Codex (ChatGPT desktop app, Codex
mode) as a manual, read-only reviewer when useful. Don't build an automated
bridge yet. Revisit the official Codex plugin for Claude Code as an opt-in
pilot once Q-002 is answered. Reasoning and the full comparison in
[research/ai-bridge.md](research/ai-bridge.md).
