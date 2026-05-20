# Example: GREEN — small bug fix

## Prompt

> Fix the typo in the navbar label.

## Session Saver response

Risk: GREEN

No Session Saver check needed. One file, clear target, no tests or repo-wide
search required. The agent proceeds normally:

1. Locate the navbar label.
2. Fix the typo.
3. Confirm the change.

## Why this is GREEN

- Single file, known location.
- No architecture decisions.
- No repeated test/debug loops.
- Low chance of long terminal output.

GREEN tasks should not be slowed down by a preflight check. Just do them.
