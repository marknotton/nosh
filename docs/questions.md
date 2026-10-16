# Open questions

Questions waiting on a decision. When one is answered, record the answer in
[decisions.md](decisions.md) and delete it here.

Format: who it's waiting on, the question, options, and a recommendation.

---

## Q-001: Approve the collaboration setup (D-001 to D-005)?

- Waiting on: Mark
- Recommendation: approve as drafted, then change whatever doesn't fit after
  the first real feature.

## Q-002: Does Codex CLI work on ChatGPT Go, and how much usage does it get?

- Waiting on: Mark (a two-minute check on the Mac)
- Why it matters: decides whether the Codex plugin pilot (D-005) is worth trying.
  OpenAI's help centre says Codex is included in Go and lists CLI sign-in, but
  the pricing page only describes Go as "lightweight coding tasks" in the
  desktop app and publishes no limits.
- How to check: in Terminal run `codex login status`, then start `codex` in
  the Nosh folder and run `/status` to see remaining usage.

## Q-003: Where should GPT's planning conversations write to the docs?

- Waiting on: Mark
- Options:
  - **Codex mode in the ChatGPT desktop app** writes the files directly. Uses
    Codex allowance on Go. *(Recommended)*
  - **Regular ChatGPT chat** produces Markdown and Mark pastes it in. No Codex
    allowance used, more copying.

## Q-004: Should `main` be protected, with all changes via pull requests?

- Waiting on: Mark
- Recommendation: yes, once building starts. It gives a natural review point
  for Codex and avoids two tools writing to the same branch.
