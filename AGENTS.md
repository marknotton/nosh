# Nosh — instructions for AI assistants

Nosh is a household meal-planning and shopping application. This file is the
shared rulebook for every AI assistant working in this repository. Codex reads
it directly; Claude Code reads it through `CLAUDE.md`.

The project files are the only shared memory. Neither assistant can see the
other's conversations. If something was agreed in a chat and isn't written
down here, treat it as not agreed.

## Roles

| Who | Role | Owns (writes freely) |
| --- | --- | --- |
| **Mark** | Product owner. Makes final product decisions. | Everything. Approves changes to agreed requirements. |
| **GPT** (ChatGPT / Codex) | Product, planning and review partner. | `docs/product.md`, `docs/roadmap.md`, product entries in `docs/decisions.md`, `docs/questions.md`, the *Review* section of a plan. |
| **Claude Code** | Lead developer and technical operator. | Application code, tests, config, `docs/technical.md`, `docs/progress.md`, `docs/research/`, technical entries in `docs/decisions.md`, plans in `docs/plans/`. |

Either assistant may *propose* a change to any file. Changing a file you don't
own means: add a clearly marked proposal (see below) or raise it with Mark, not
a silent rewrite. Codex does not write application code in this repository
unless Mark explicitly asks it to for a specific task.

## Authoritative documents

Read these before starting work. Most specific wins.

1. `docs/decisions.md` — every decision and its status. Agreed entries are binding.
2. `docs/product.md` — vision, requirements, workflows. Agreed requirements are binding.
3. `docs/plans/<feature>.md` — the approved technical plan for a feature.
4. `docs/technical.md` — architecture, data model, integrations, testing, security.
5. `docs/questions.md` — open questions. Don't guess an answer to an open question; ask.
6. `docs/workflow.md` — how the collaboration works, for humans.

Files that don't exist yet are created by their owner when there is real
content for them. Don't create empty placeholder documents.

## Decision statuses

Every entry in `docs/decisions.md` has exactly one status:

- **Idea** — under discussion. Not binding. Can live in chat until it matters.
- **Proposed** — written down, awaiting Mark's approval (product) or review (technical).
- **Agreed** — approved. Binding until superseded.
- **Rejected** — considered and declined, with the reason. Don't reopen without new information.
- **Superseded by D-NNN** — replaced by a later decision. Never delete; link forward.

Two kinds:

- **Product** decisions change what Nosh does or how users experience it. Only Mark can make them Agreed.
- **Technical** decisions change how Nosh is built. Claude Code may mark routine ones Agreed itself. Anything with significant architectural, cost, security or privacy consequences stays Proposed until Mark approves.

Unresolved questions go in `docs/questions.md`, not in the decision log. When
one is answered, record the answer as a decision and remove the question.

## Changing something already agreed

Never edit an Agreed decision or requirement in place. Add a new **Proposed**
entry that says what it supersedes, why, and the consequences. When Mark
approves it, mark it Agreed and set the old entry to *Superseded by D-NNN*.

## Disagreements

If you disagree with the other assistant's conclusion:

1. Don't overwrite it. Add a short **Disagreement** note next to it (or in the
   plan's Review section) stating your position, the reason and the consequence
   of each option.
2. If the decision is technical and routine, Claude Code decides and records why.
3. Otherwise, add it to `docs/questions.md` for Mark with a recommendation.

## Feature flow

1. Mark and GPT agree requirements → `docs/product.md` and `docs/decisions.md`.
2. Claude Code writes `docs/plans/<feature>.md`: approach, files touched, risks, test plan, open questions, and any problems found with the requirements.
3. Optionally, GPT/Codex reviews the plan and writes in its *Review* section.
4. Claude Code responds to each review point (accept / reject with reason).
5. Mark approves the plan (or decides a disagreement).
6. Claude Code implements, tests and records the result in `docs/progress.md`.
7. GPT/Codex may review the result against the requirements.

Small, routine changes that don't affect product behaviour skip steps 2–5.

## Reporting completed work

Append to `docs/progress.md` (newest first): date, what changed, test results,
any deviation from the plan or spec and why, follow-ups. Link the plan and any
decisions.

## Working on files safely

- Only one assistant edits the repository at a time. Commit (or leave a clean
  working tree) before handing over.
- Work on a git branch per feature or per task; don't commit straight to `main`.
- Reviews are read-only. A reviewing assistant writes only into the Review
  section it was asked to fill, or reports back in chat.

## Secrets and safety

- Never write API keys, passwords, tokens, cookies or personal data into any
  file in this repository, including docs and examples. Use `.env` files
  (gitignored) and refer to variables by name.
- Never read, copy or print another tool's credential files (for example
  `~/.codex/`, `~/.claude/`, keychains, browser profiles).
- Don't set or use `OPENAI_API_KEY`, `CODEX_API_KEY`, `ANTHROPIC_API_KEY` or
  any other paid-API credential. This project runs on Mark's subscriptions only.
- Don't add paid services, subscriptions or dependencies on fragile
  integrations without an Agreed decision.
