# Example: RED — large refactor

## Prompt

> Refactor the entire auth system and add tests.

## Session Saver response

```md
## Session Saver Check

Risk: RED

Why:
- This likely touches multiple files.
- It may require understanding the existing auth flow first.
- It may require repeated test/debug loops.
- Starting directly increases the risk of unfinished work.

Recommended approach:
1. Inspect the auth flow without editing.
2. Identify the smallest safe refactor.
3. Refactor one module.
4. Add focused tests.
5. Run targeted tests and summarize.

Suggested next prompt:
"Inspect the auth flow and produce a concise implementation plan. Do not edit files yet."
```

## Suggested slices

```md
## Task Slices

1. Goal: Map the current auth flow and write a short plan.
   Files: auth/ (read only)
   Definition of done: A concise plan listing modules and refactor order.

2. Goal: Refactor one auth module per the plan.
   Files: the single module + its direct callers
   Definition of done: Module refactored, existing behavior unchanged.

3. Goal: Add focused tests for the refactored module.
   Files: the module's test file
   Definition of done: Targeted tests pass.
```

## Why this is RED

- Many files and unknown exact scope.
- Auth is a sensitive area.
- Likely repeated test/debug loops.

RED tasks should be split before any editing. Inspect first, then refactor one
module at a time.
