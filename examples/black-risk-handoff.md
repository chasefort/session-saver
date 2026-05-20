# Example: BLACK — session too messy

## Prompt

> We have been working for hours. Finish the migration, fix all tests, clean the
> repo, and make it production ready.

## Session Saver response

```md
## Session Saver Check

Risk: BLACK

Why:
- The conversation is already long and context is messy.
- The request bundles several large, unrelated tasks.
- High risk of compaction or reset mid-task.
- Likely to produce half-finished edits across the repo.

Recommended approach:
1. Stop adding new work to this session.
2. Generate a handoff capturing current state.
3. Start a fresh session and resume from the handoff, one task at a time.

Suggested next prompt:
"Generate a session handoff for the migration work, then I will start a fresh
session and tackle the migration first."
```

## Handoff produced

```md
# Session Handoff

## Goal
Complete the database migration, then stabilize tests.

## Current Status
Migration is ~60% done; new schema applied, data backfill incomplete.

## Files Touched
- `db/migrations/0042_user_schema.sql`: new schema, backfill not finished.
- `models/user.ts`: updated to new fields.

## Important Decisions
- Decision: Backfill in a separate step from schema change.
- Reason: Keeps the schema migration fast and reversible.

## Failed Attempts / Do Not Repeat
- Attempt: Backfilling inside the schema migration.
- Why it failed: Locked the table too long on large datasets.

## Next 3 Steps
1. Finish the data backfill script.
2. Run the migration on a staging copy.
3. Re-run the test suite and triage failures.

## Current Risks
- Risk: Partial backfill leaves rows in an inconsistent state.
- What to check: Row counts before and after the backfill.

## Resume Prompt
Paste this into a fresh session:

"Resume the database migration. Schema is applied; the data backfill is
incomplete. Finish the backfill script, run it on staging, then re-run tests.
Do not touch unrelated cleanup."
```

## Why this is BLACK

- Long session, messy context, likely compaction.
- Multiple unrelated large asks bundled together.
- Continuing here risks losing everything to an unfinished mess.

BLACK tasks call for a handoff and a fresh session — not more work in the
current one.
