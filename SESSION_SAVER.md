# Session Saver

A startup skill for coding agents. Before risky work, classify the task and decide
whether it can finish cleanly in this session — then proceed, split, or hand off.

This is risk judgment, not token prediction. The goal: finish one clean slice
instead of losing a whole session to an unfinished mess.

---

## Classify every coding task

Before executing a coding request, classify it as one of:

- **GREEN** — small, local, clear target. Finishable now.
- **YELLOW** — medium scope, 2–5 files. Plan briefly, execute one slice.
- **RED** — broad or risky. Split into slices before editing.
- **BLACK** — too large, vague, or context-heavy. Hand off or start fresh.

GREEN: proceed normally, no check needed.
YELLOW / RED / BLACK: show the Session Saver check below before doing work.

---

## Risk signals

Treat a request as RED or BLACK when it involves:

- repo-wide analysis or large refactors
- migrations; auth, payments, permissions, data models, infrastructure
- many files or unknown file scope
- vague goals: "fix everything", "clean this up", "make it production ready"
- likely repeated test/debug loops or long terminal output
- an already-long conversation, prior compaction, or earlier failed attempts

---

## Session Saver check

For YELLOW / RED / BLACK tasks, respond with:

```md
## Session Saver Check

Risk: GREEN | YELLOW | RED | BLACK

Why:
- ...

Recommended approach:
1. ...

Suggested next prompt:
"..."
```

Keep it short. Don't turn the check into a full plan unless asked.
Always end with a copy-pasteable next prompt.

---

## Execution rules

- Do not start RED or BLACK tasks directly.
- RED: propose 2–5 independently finishable slices (see task slice format).
- BLACK: recommend a fresh session, a handoff, or a narrower request.
- Prefer one complete slice over many half-finished edits.
- Avoid reading unnecessary files and verbose terminal output.
- Run targeted commands instead of broad ones.
- Summarize state before context gets messy.
- When unsure, reduce scope.

---

## Task slice format

When a task is too large, split it:

```md
## Task Slices

1. Goal:
   Files:
   Definition of done:

2. Goal:
   Files:
   Definition of done:
```

Each slice must be independently finishable.

---

## Handoff

Create a handoff when the task is unfinished, the conversation is long, context is
messy, compaction is likely, or the user is switching sessions or agents.

```md
# Session Handoff

## Goal
## Current Status
## Files Touched
## Important Decisions
## Failed Attempts / Do Not Repeat
## Next 3 Steps
## Current Risks
## Resume Prompt
```

The Resume Prompt must let the user continue in a fresh session by pasting it in.

---

## Guiding question

> Can this task finish cleanly in the current session?

Yes → proceed. No → slice it or hand off.
