# How the AI collaboration works

Status: **Proposed** (see D-001 to D-005 in [decisions.md](decisions.md)).

This is the human guide. The rules the assistants follow are in
[`AGENTS.md`](../AGENTS.md); this page doesn't repeat them.

## The short version

- **You** decide what Nosh does.
- **GPT** helps you work out what that is, and writes it down.
- **Claude Code** works out how to build it, builds it, and writes down what it did.
- **The repo's Markdown files** are how the two assistants talk. Nothing said
  only in a chat counts.

## Documentation structure

Start small. A file is created when it first has real content, not before.

| File | Owner | Holds | Exists now? |
| --- | --- | --- | --- |
| `AGENTS.md` | Mark (both propose) | Rules for all assistants: roles, authority, statuses, safety. | Yes |
| `CLAUDE.md` | Claude | Imports `AGENTS.md`, adds lead-developer rules. | Yes |
| `docs/workflow.md` | Claude | This guide. | Yes |
| `docs/decisions.md` | Both | Decision log, product and technical, with statuses. Doubles as the ADR log. | Yes |
| `docs/questions.md` | Both | Open questions waiting on someone, with a recommendation. | Yes |
| `docs/research/` | Claude | Technical research notes (one file per topic). | Yes (AI bridge) |
| `docs/product.md` | GPT | Vision, functional and non-functional requirements, user workflows, acceptance criteria. | When planning starts |
| `docs/roadmap.md` | GPT | Milestones and order of work. | When there's a first milestone |
| `docs/plans/<feature>.md` | Claude (GPT writes the Review section) | Implementation plan per feature, plus review and responses. | Per feature |
| `docs/technical.md` | Claude | Architecture, data model, integrations, testing approach, security and privacy. | When the stack is chosen |
| `docs/progress.md` | Claude | Implementation log: what changed, test results, deviations. | When building starts |

When a file grows too big, split it into a folder (for example
`docs/product.md` → `docs/product/requirements.md`, `docs/product/workflows.md`).
Your full wish-list maps like this:

| Topic | Lives in |
| --- | --- |
| Product vision, functional and non-functional requirements, UX and workflows | `docs/product.md` |
| Testing and acceptance criteria | Acceptance criteria in `docs/product.md`; test approach in `docs/technical.md`; results in `docs/progress.md` |
| Technical architecture, data model, third-party integrations, security and privacy | `docs/technical.md` |
| Architecture decision records | `docs/decisions.md` |
| Roadmap | `docs/roadmap.md` |
| Outstanding questions | `docs/questions.md` |
| Technical research | `docs/research/` |
| Implementation progress | `docs/progress.md` |

## A typical feature, in practice

1. **Plan with GPT.** Chat in ChatGPT as normal. When you've agreed something,
   ask GPT to write it into the docs. Two ways:
   - Easiest: switch to **Codex mode** in the ChatGPT desktop app, pointed at
     the Nosh folder, and say *"Record what we agreed about X in
     docs/product.md and docs/decisions.md."* Codex reads `AGENTS.md`, so it
     knows the format and its limits. Commit when it's done.
   - Fallback: ask ChatGPT for the Markdown and paste it in yourself.
2. **Hand to Claude.** In Claude Code: *"Plan feature X."* Claude reads the
   docs and writes `docs/plans/x.md`, flagging any problems with the requirements.
3. **Optional review.** In Codex mode: *"Review docs/plans/x.md against
   docs/product.md. Write your findings in its Review section only."*
4. **Back to Claude.** *"Respond to the review on docs/plans/x.md."* Claude
   accepts or rebuts each point and lists anything that needs you.
5. **You decide** only what's listed as needing you, then say *"approved"*.
6. **Claude builds it**, runs tests and updates `docs/progress.md`.
7. **Optional acceptance check.** In Codex mode: *"Check the latest entry in
   docs/progress.md against the requirements for X."*

Your role as messenger shrinks to saying *"go look at file Y"*. You never copy
content between the assistants.

## What is and isn't automated

| Step | Automated? |
| --- | --- |
| Context transfer between assistants | Yes, via the shared files. No copy-paste. |
| Starting each assistant | No. You open the app and give a one-line prompt. |
| GPT reviewing Claude's plan | Manual today (you ask Codex). Could become semi-automatic with the Codex plugin, see D-005. |
| Claude reading GPT's requirements | Yes, automatic at the start of each task. |
| Approving product changes | Never automated. Always you. |

Why not more? See [research/ai-bridge.md](research/ai-bridge.md).

## Rules of thumb

- One assistant edits the repo at a time. Commit before switching apps.
- If you agreed something in a chat, it isn't real until it's in `decisions.md`.
- If a question keeps coming back, check `decisions.md` for a Rejected or Agreed entry first.
