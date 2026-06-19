---
name: feedback_dont_manufacture_wait_states
description: Verify a gate/cron/event is real before deferring; a manual trigger usually replaces a calendar wait. Audit what's doable now.
metadata:
  type: feedback
---

When something "needs to wait" for a gate, cron, or external event, verify the gate is
**real** before deferring. Often a manual trigger replaces the wait, or the dependency
isn't actually blocking.

**Why:** Phantom wait-states quietly stall work: "we'll do it next session / after the
next run" when it could be done now. The original failure mode of deferred work is that it
silently never happens. **How to apply:** When tempted to defer, first audit what's doable
*right now*: can the job be triggered manually? is the gate verified to exist (check the
schedule/state, don't trust the doc)? Only defer what genuinely depends on elapsed time or
an external party. When you do, name the concrete trigger/date, not "later". Pairs
with [[feedback_scope_claims_to_evidence]].
