# Auto-Continue

Automatically restart Claude Code after a session ends — whether from context
limits, compaction, or a rate-limit pause — with no manual intervention.

---

## How it works

1. When session health drops, the agent writes `.session-continue` to your project root
   and saves a checkpoint to `HANDOFF.md`.
2. A Claude Code **Stop hook** detects that file and runs `claude --continue`.
3. The new session reads the checkpoint and picks up where you left off.

The hook is conditional — it only fires when the agent explicitly signals it needs
to continue. It does **not** loop on every stop.

---

## Setup

### 1. Add the Stop hook

Add to `.claude/settings.json` in your project (or `~/.claude/settings.json` globally):

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c '[ -f .session-continue ] && rm .session-continue && claude --continue || true'"
          }
        ]
      }
    ]
  }
}
```

If you already have a `hooks` block, merge the `Stop` key in — don't replace it.

### 2. Add `.session-continue` to `.gitignore`

```
.session-continue
```

### 3. Reference in CLAUDE.md

```
@SESSION_SAVER.md
@AUTO_CONTINUE.md
```

That's it. The agent will handle the rest.

---

## Rate-limit resilience

When Claude Code is rate-limited, the Stop hook exits silently (the `claude --continue`
call fails and the `|| true` swallows the error). To keep retrying until credits
refresh, run this once in your terminal:

```bash
until claude --continue; do
  echo "Rate limited — retrying in 5 min..."
  sleep 300
done
```

Or save it as a shell alias so it's always one command away:

```bash
# Add to ~/.zshrc or ~/.bashrc
alias cc='until claude --continue; do echo "Rate limited — retrying in 5 min..."; sleep 300; done'
```

Then run `cc` in your terminal whenever you want resilient continuation.

---

## What the agent saves before signaling

- `.session-continue` — the trigger file (deleted by the hook on restart)
- `HANDOFF.md` — goal, status, files touched, decisions, failed attempts, next steps

The **Resume Prompt** inside `HANDOFF.md` gives the new session everything it needs.

---

## Honest limits

- This cannot predict when your rate limit refreshes — the retry loop polls until success.
- `claude --continue` resumes the **most recent** session. If you have multiple projects
  open, run it from the right directory.
- If the session was compacted, the new session starts from the compressed summary.
  The `HANDOFF.md` checkpoint fills in what compression lost.
- The Stop hook requires Claude Code v1.x with hooks support. Check with `claude --version`.
