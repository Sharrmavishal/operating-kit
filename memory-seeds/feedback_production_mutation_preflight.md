---
name: feedback_production_mutation_preflight
description: Before any irreversible shared-state write, answer five checks in the response first.
metadata:
  type: feedback
---

Before ANY irreversible shared-state write (DB write, TRUNCATE, destructive `--apply`,
deploy), answer all five **in the response, before running**:
1. **Backup** — a known-good restore point exists and I can state its timestamp.
2. **Ownership** — which job/code path *actually* writes this in prod (verified), not the
   obviously-named one.
3. **Blast radius** — what this transitively cascades / overwrites (run the dependents
   query *before* the write, not during recovery).
4. **Mechanism, not just inputs** — the *mutating* path has been exercised on a copy, not
   only the read/select path.
5. **Memory check** — re-read notes/memories about *this subsystem* against *this* op.

**Why:** Irreversible writes to shared state are where the expensive incidents live; the
cost of the five checks is minutes, the cost of skipping them is a restore (or worse, no
restore). **How to apply:** Make it a visible checklist in the message, not a silent
intention. Pairs with [[feedback_scratch_test_before_prod]] and
[[feedback_enforce_safety_in_code_not_discipline]].
