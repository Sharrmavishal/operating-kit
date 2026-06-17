---
name: feedback_check_premise_first
description: Ground-truth a brief's premise against the code/data before executing; many briefs rest on an assumption the system contradicts.
metadata:
  type: feedback
---

Before executing a brief/plan, confirm its premise is true against the actual code and
data. Lead the reply with a premise check, then evidence, then recommendation.

**Why:** Briefs are often built on an assumption that the codebase or data quietly
contradicts (a field that doesn't exist, a job that never runs, a "frozen" thing that's
actually fine, a "broken" thing that's intentional). Building on the unchecked premise
wastes the whole effort. **How to apply:** Five minutes of `grep`/query/log-read to verify
the premise beats a day building on sand. If the premise is wrong, say so with the
evidence and propose the corrected framing before doing the work. Pairs with
[[feedback_verify_dont_infer_from_output]] and [[feedback_surface_reframe_with_options]].
