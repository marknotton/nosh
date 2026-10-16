# Open questions

Questions waiting on a decision. When one is answered, record the answer in
[decisions.md](decisions.md) and delete it here.

Format: who it's waiting on, the question, options, and a recommendation.

---

## Q-002: Does Codex CLI work on ChatGPT Go, and how much usage does it get?

- Waiting on: Mark (local check on the Mac)
- Why it matters: decides whether the Codex plugin pilot (D-005) is worth trying.
  OpenAI's help centre says Codex is included in Go and lists CLI sign-in, but
  the pricing page only describes Go as "lightweight coding tasks" in the
  desktop app and publishes no limits.
- How to check: in Terminal run `codex login status`, then start `codex` in
  the Nosh folder and run `/status`. Confirm it shows a ChatGPT login (not an
  API key) and what allowance is available.
- Constraint: don't install Codex CLI or the Claude Code plugin without Mark's
  approval.
