# Example: YELLOW — small feature

## Prompt

> Add a new settings toggle to the dashboard.

## Session Saver response

```md
## Session Saver Check

Risk: YELLOW

Why:
- Likely touches UI, state, and possibly tests (2–5 files).
- Scope is clear, but it is more than a one-file edit.

Recommended approach:
1. Add the toggle UI to the dashboard settings panel.
2. Wire it to the existing settings state.
3. Add a focused test for the new toggle.

Suggested next prompt:
"Add the settings toggle UI and wire it to settings state. Keep it to one slice;
do not refactor the surrounding panel."
```

## Why this is YELLOW

- Spans a few related files (UI + state + test).
- Clear success criteria.
- Moderate scope — plan briefly, then execute one focused slice.

YELLOW tasks are finishable now, but benefit from a short plan so the agent does
not drift into a refactor.
