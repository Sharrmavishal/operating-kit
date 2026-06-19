---
name: feedback_canonical_state_doc_not_memory
description: Keep one canonical state doc for live state (versions, deployed revision, open issues); never trust conversation memory for it. Checkpoint before risky/large work; /clear with the state doc so you can resume.
metadata:
  type: feedback
---

Keep **one canonical state doc** as the source of truth for live state (current versions,
deployed revision, known issues, what's next) and update it at the end of every significant
session. Never trust conversation memory or a doc's "added/deployed" claim for live state;
confirm against the running system. Checkpoint (commit/save) before risky or large work so a
crash or a bad turn can't lose progress.

**Why:** A long session drifts from reality: what you "remember" deployed, what the doc
*claims* is live, and what's *actually* running diverge silently. The cost surfaces as a
stale-state mistake (acting on a version that isn't deployed, re-doing work, missing a known
issue). A canonical doc + a live re-check at session start kills that class.

**How to apply:** At session start, read the state doc and verify one cheap live signal
(version endpoint / deployed revision). Flag any mismatch loudly. At session end, update the
doc with targeted edits (don't rewrite it). Long context? `/clear` is safe *because* the state
doc lets the next session resume. The `session-start` / `session-end` agents automate this.
Pairs with [[feedback_verify_dont_infer_from_output]] and [[feedback_scope_claims_to_evidence]].
