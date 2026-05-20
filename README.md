# Session Saver

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Markdown only](https://img.shields.io/badge/install-none-brightgreen)](SESSION_SAVER.md)
[![Works with Claude Code](https://img.shields.io/badge/Claude%20Code-✓-blue)](https://claude.ai/code)

**Stop giving coding agents tasks they can't finish.**

A one-file Markdown skill that helps coding agents decide whether a task can
finish cleanly in the current session — then proceed, split it, or generate a
handoff.

No install. No server. No dashboard. One Markdown file.

---

## The problem

You give a coding agent a big task. It starts confidently, reads too many files,
runs verbose commands, edits half the repo, hits compaction or a rate limit — and
leaves you with a half-finished mess and a wasted session.

The real frustration isn't _"I don't know how many tokens I have left."_ It's:

> I don't know whether this task is safe to start right now.

Most usage tools answer **"how much have I used?"** Session Saver answers
**"should I start this task, split it, or save state first?"**

---

## What it does

Before risky work, the agent classifies the task into a risk level:

| Level | Meaning | Behavior |
|-------|---------|----------|
| 🟢 **GREEN** | Small, local, clear target | Proceed normally |
| 🟡 **YELLOW** | Medium scope, 2–5 files | Plan briefly, do one slice |
| 🔴 **RED** | Broad or risky | Split into slices before editing |
| ⬛ **BLACK** | Too large or messy | Hand off or start a fresh session |

For anything past GREEN, the agent surfaces a short check — why it's risky, a
recommended approach, and a copy-pasteable next prompt — instead of charging in.

---

## Quick start

**1.** Copy [`SESSION_SAVER.md`](SESSION_SAVER.md) into your project root.

**2.** Tell your agent to read it before starting tasks:

| Tool | How |
|------|-----|
| **Claude Code** | Add `@SESSION_SAVER.md` to your `CLAUDE.md` |
| **Codex** | Add `@SESSION_SAVER.md` to your `AGENTS.md` |
| **Cursor** | Paste into `.cursorrules` or project rules |
| **Gemini CLI** | Reference in your `GEMINI.md` |
| **Any agent** | Paste it into your system prompt or agent instructions |

That's it. The file is short by design — agents read it on every startup without
burning meaningful context.

---

## Example

**Prompt:**

```
Refactor the whole auth system and add tests.
```

**Session Saver response:**

```md
## Session Saver Check

Risk: RED

Why:
- This likely touches multiple files across the auth layer.
- It requires understanding the existing flow before editing.
- It will need repeated test/debug loops.
- Starting directly increases the risk of an unfinished session.

Recommended approach:
1. Inspect the auth flow without editing.
2. Identify the smallest safe refactor.
3. Refactor one module.
4. Add focused tests for that module.
5. Run targeted tests and summarize.

Suggested next prompt:
"Inspect the auth flow and produce a concise implementation plan. Do not edit files yet."
```

You get an immediate, safe next action instead of a blown session.

---

## Risk levels

| Level | Signals |
|-------|---------|
| 🟢 **GREEN** | One file, clear target, small fix. Proceed. |
| 🟡 **YELLOW** | 2–5 files, a small feature or focused refactor. Plan, do one slice. |
| 🔴 **RED** | Many files, migrations, auth/payments/data models, vague "make it better" requests, likely test loops. Split before editing. |
| ⬛ **BLACK** | Already-long session, prior compaction, multiple failed attempts, many unrelated asks. Hand off and start fresh. |

---

## Included files

| File | Purpose |
|------|---------|
| [`SESSION_SAVER.md`](SESSION_SAVER.md) | The runtime skill — copy this into your project |
| [`HANDOFF_TEMPLATE.md`](HANDOFF_TEMPLATE.md) | Preserve state before a reset or tool switch |
| [`TASK_SLICE_TEMPLATE.md`](TASK_SLICE_TEMPLATE.md) | Break a big task into finishable slices |
| [`examples/`](examples/) | One worked example per risk level |

---

## Honest limits

Session Saver does **not** predict token usage or session limits. It can't —
usage depends on model, tool calls, file sizes, terminal output, test loops,
hidden reasoning, compaction, conversation length, and provider quotas.

It makes practical **risk judgments**, not precise predictions. Expect language
like "likely finishable" and "safer to split" — never "guaranteed completion."

---

## Philosophy

> Finish one clean slice instead of losing a whole session to an unfinished mess.

Finish cleanly. Split risky work. Preserve context. Generate handoffs.

---

## Contributing

This project is intentionally Markdown-only. PRs that keep it that way are
welcome — better heuristics, sharper examples, more agent-specific setup guides.
Keep `SESSION_SAVER.md` under 150 lines.

---

## License

[MIT](LICENSE)
