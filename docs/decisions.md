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

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16), proposed by Claude

Markdown files in this repository are the only shared memory between GPT and
Claude Code. Chat history is for exploration. Neither assistant can read the
other's conversations, so anything that matters is written down.

## D-002: Division of responsibilities

- Status: Agreed
- Type: Product
- Date: 2026-10-16
- Decided by: Mark (2026-10-16), proposed by Claude

Mark is product owner. GPT owns product requirements, product decisions and
the roadmap. Claude Code owns implementation, tests and technical records.
Either may propose changes anywhere; changes to agreed requirements need
Mark's approval. Details and file ownership in `AGENTS.md`.

## D-003: Minimal documentation structure, grown on demand

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16), proposed by Claude

Start with `AGENTS.md`, `CLAUDE.md`, `docs/workflow.md`, `docs/decisions.md`,
`docs/questions.md` and `docs/research/`. Create `product.md`, `roadmap.md`,
`plans/`, `technical.md` and `progress.md` when they first have content. One
decision log doubles as the ADR record rather than one file per ADR, until it
gets unwieldy.

Alternative considered: a full skeleton up front. Rejected because empty files
go stale and make it unclear what's real.

## D-004: Shared instructions in AGENTS.md, Claude imports them

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16), proposed by Claude

Codex reads `AGENTS.md` natively. Claude Code reads `CLAUDE.md`, which imports
`AGENTS.md` with `@AGENTS.md` and adds Claude-only rules. One copy of the shared
rules, no duplication.

## D-005: GPT ↔ Claude communication approach

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16), proposed by Claude

Staged approach. Use the shared-document workflow now, with Codex (ChatGPT
desktop app, Codex mode) as an optional, manual, read-only reviewer. Don't
build an automated bridge yet. The official Codex plugin for Claude Code is a
conditional, opt-in pilot: only after Q-002 confirms subscription auth works on
Go with a usable allowance and no separately billed API use, and only if
testing shows it's worth it. Nothing (Codex CLI or the plugin) is installed
without Mark's approval. If Go's allowance is too limited or the bridge adds
friction, stay with shared documents and manual reviews. Reasoning and the
full comparison in [research/ai-bridge.md](research/ai-bridge.md).

## D-006: GPT records decisions via Codex mode

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16)

GPT explores ideas and agrees requirements with Mark in normal ChatGPT
conversations. When something needs recording, Mark asks Codex (Codex mode in
the ChatGPT desktop app, with the Nosh repository open) to update the relevant
documents directly. Fallback: ChatGPT produces Markdown and Mark pastes it in.
The one-writer rule in `AGENTS.md` applies: commit or hand over cleanly before
switching assistants.

## D-007: Protect `main` once application development starts

- Status: Agreed
- Type: Technical
- Date: 2026-10-16
- Decided by: Mark (2026-10-16)

From the start of application development, `main` is protected and all changes
arrive through feature branches and pull requests. PRs are the natural review
point, especially for substantial features and architectural changes. The exact
configuration is D-008.

## D-008: Branch protection configuration for `main`

- Status: Proposed
- Type: Technical
- Date: 2026-10-16
- Decided by: Claude (proposer)

What's available: the repository is **public** on a personal GitHub Free
account. GitHub's protected branches and rulesets are available for public
repositories on Free, so no paid plan is needed.

Recommended: one repository ruleset targeting `main` (the default branch), with:

- **Require a pull request before merging**, with **0 required approvals**.
  Mark is the only human with write access, and GitHub doesn't let a PR's
  author approve their own PR, so requiring an approval would block every
  merge. The PR itself is the review point; Mark merges when satisfied.
- **Block force pushes** and **restrict deletions**.
- **No required status checks yet.** Add them when CI exists.
- **No bypass list.** With 0 approvals Mark can still merge his own PRs, so
  nobody needs to bypass, and assistants can't push to `main` directly.

Not recommended (overhead without benefit for a one-person project): required
signed commits, linear history, CODEOWNERS, merge queues, deployment gates.

Prerequisite: set `main` as the repository's default branch (it is currently
the setup branch, because the repository was empty when it was first pushed).
Applied by Mark, or by Claude with Mark's go-ahead, when development starts.
