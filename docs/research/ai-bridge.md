# Research: how GPT and Claude Code can communicate

Checked: 2026-10-16. Subscriptions in scope: **ChatGPT Go** and **Claude Pro**.
No API keys, no paid add-ons.

## Findings from primary sources

1. **Codex is included in ChatGPT Go.** "Codex is included across ChatGPT
   plans, including Free and Go. Usage limits vary by plan."
   ([OpenAI Help: Codex](https://help.openai.com/en/articles/11369540-codex))
2. **But Go's allowance is small and unpublished.** The pricing page describes
   Go as "Use Codex for lightweight coding tasks", names only the desktop app,
   and gives no numbers. CLI/IDE availability for Go isn't spelled out there.
   ([Codex pricing](https://learn.chatgpt.com/docs/pricing.md)) → Q-002.
3. **Sign in with ChatGPT is supported in the desktop app, CLI and IDE
   extension.** API-key sign-in bills at API rates instead; we avoid it.
   The CLI and IDE extension share a cached login in `~/.codex/auth.json` or
   the OS keychain, which OpenAI says to treat like a password.
   ([Codex auth](https://learn.chatgpt.com/codex/auth))
4. **Scripted use with ChatGPT login is allowed, with caveats.** `codex exec`
   reuses saved CLI auth and defaults to a read-only sandbox. OpenAI recommends
   API keys for automation and calls ChatGPT-managed auth in automation an
   "advanced" path for trusted machines, "only if you specifically need to run
   as your Codex account", and not for public repositories.
   ([Non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode.md))
5. **OpenAI ships an official Codex plugin for Claude Code.** It wraps the
   local Codex CLI and "uses your local Codex CLI authentication"; requirements
   are "ChatGPT subscription (incl. Free) or OpenAI API key" and Node 18.18+.
   `/codex:review` and `/codex:adversarial-review` are read-only. `/codex:rescue`
   can change code. Its optional *review gate* "can create a long-running
   Claude/Codex loop and may drain usage limits quickly."
   ([openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc))
6. **Claude Code doesn't read `AGENTS.md`**, but a `CLAUDE.md` containing
   `@AGENTS.md` imports it, so both tools share one rulebook.
   ([Claude Code memory docs](https://code.claude.com/docs/en/memory))
7. **ChatGPT's GitHub connector is plan-dependent and read-only.** "GitHub
   availability can vary by plan, workspace, and product surface." Go isn't
   named. ([Connecting GitHub to ChatGPT](https://help.openai.com/en/articles/11145903-connecting-github-to-chatgpt))
8. **Nothing gives either assistant access to the other's chat history or
   memory.** Files are the only bridge.

## Options compared

| | A. Shared docs | B. Codex as manual reviewer | C. Automated local bridge | D1. GitHub as handoff | D2. Codex MCP server | D3. UI automation |
| --- | --- | --- | --- | --- | --- | --- |
| **Enables** | Both assistants work from the same files; Mark starts each one | Independent second opinion on plans and diffs | Claude asks Codex for a review mid-task, no app switching | PRs/branches as the review unit; ChatGPT reads repo | Claude calls Codex as a tool | Scripted driving of the ChatGPT app |
| **Setup** | These docs | None beyond A; Codex mode pointed at the Nosh folder | Install Codex CLI, `codex login`, add plugin in Claude Code | GitHub repo (exists), connectors | `codex mcp-server` registered in Claude Code | Accessibility scripting |
| **Works on Go + Pro?** | Yes | Yes (within Go's Codex allowance) | Probably; confirm CLI on Go (Q-002) | Partly; connector on Go unconfirmed | Same as C | Technically |
| **Extra API charges?** | No | No | No, *if no OpenAI API key is set in the environment* | No | No (same caveat) | No |
| **Local files?** | Yes | Yes | Yes | Repo only, via GitHub | Yes | n/a |
| **Risks** | Relies on discipline to write things down | Uses Go's small allowance | Shares Mark's ChatGPT session with a subprocess; OpenAI labels this advanced; feedback loops drain limits; only works in *local* Claude Code sessions, not cloud ones | Read-only in ChatGPT; another moving part | Lower-level than the plugin, more to maintain | Fragile, likely against terms, breaks on every app update |
| **Worth it now?** | **Yes** | **Yes, when a plan is non-trivial** | **Later, as an opt-in pilot** | Once building starts (Q-004) | No, plugin covers it | **No** |

Reverse direction (Codex calling Claude Code via `claude -p` with the Pro
login) is possible but pointless here: Claude is the lead, so Claude should be
the one asking for reviews.

## Recommendation

**A + B now. C later, maybe.**

- A gets ~90% of the benefit: you stop copying content and only say "go read
  file X". It costs nothing and can't break.
- B costs a little Codex allowance only when you choose to use it.
- C is supported and doesn't involve API billing, which is better than I
  expected. But it isn't worth it until (1) Go is confirmed to include CLI
  access with usable limits, (2) we've done one feature by hand and know a
  review step is actually wanted, and (3) you mostly run Claude Code locally,
  since the Codex login lives on your Mac and cloud sessions can't use it.

If we do pilot C:

- Install only the official plugin. Use only `/codex:review` and
  `/codex:adversarial-review`. Don't enable the review gate. Don't use
  `/codex:rescue` (it lets Codex change code, which breaks the one-writer rule).
- Make sure no `OPENAI_API_KEY` / `CODEX_API_KEY` is set, so usage stays on Go.
- Add a Claude Code permission rule denying reads of `~/.codex/**`, so Claude
  can call Codex without being able to read its token.
- Remove it if it adds friction or eats the Go allowance.
