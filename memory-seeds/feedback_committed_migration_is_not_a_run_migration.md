---
name: feedback_committed_migration_is_not_a_run_migration
description: A migration file in the repo doesn't mean it ran. Know whether your deploy applies migrations or only syncs schema: data migrations (UPDATE/DELETE/INSERT) often never auto-run and must be applied by hand, then verified with a live count.
metadata:
  type: feedback
---

A committed migration is not a run migration. Know whether your deploy path *applies* migrations
(executes the SQL) or merely *syncs schema* (adds/removes columns to match the model). If it only
syncs, any **data** migration (an UPDATE/DELETE/INSERT) silently never runs: the column exists,
the backfill didn't happen. Apply data migrations by hand (or via a one-shot script), say so in
the commit message, and confirm with a live count after deploy.

**Why:** schema diffs make a table *look* migrated, so a never-executed data step throws no error
and passes every local test. It surfaces as wrong data in production, days later. The failure is
invisible precisely because the schema half succeeded.

**How to apply:** before shipping any migration that moves data, state which mechanism your deploy
uses and how the data step gets applied. Pairs with [[feedback_verify_dont_infer_from_output]] and
[[feedback_scratch_test_before_prod]]. More traps in `docs/claude/field-notes.md`.
