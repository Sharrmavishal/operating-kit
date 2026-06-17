---
name: feedback_scratch_test_before_prod
description: Run the mutating path on a real copy, assert invariants, show the diff — before prod. A dry-run only de-risks the path it executed.
metadata:
  type: feedback
---

For anything irreversible at scale: pull a copy of production → run the **mutating** path
against the copy → assert the invariants (what must stay true) → show the human the diff
→ only then touch prod.

**Why:** A dry-run that returns the expected number only de-risks the read/select path it
executed; the destructive step may never run in dry-run, so its first real execution is in
production. A scratch copy is real prod data, so a surprise lands on the throwaway instead
of the live system. **How to apply:**
- Name the invariants up front and assert them on the copy (e.g. "X preserved, exactly N
  changed, 0 orphaned, re-run is a no-op"). All must pass before prod.
- Same copy version as prod (avoid cross-version restore surprises).
- After the prod run, the result must match the copy's result *exactly* — any divergence
  is a stop-and-look, because it means prod and the copy disagree. Pairs with
  [[feedback_production_mutation_preflight]] and [[feedback_verify_dont_infer_from_output]].
