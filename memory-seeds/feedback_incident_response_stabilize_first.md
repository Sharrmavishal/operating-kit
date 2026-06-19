---
name: feedback_incident_response_stabilize_first
description: When prod breaks, stabilize before you diagnose; name the recovery point before the first write; fix the rule, not just the instance.
metadata:
  type: feedback
---

When production breaks, follow the order — it's the whole point: **stabilize → confirm
recovery point → recover one change at a time → verify live → root-cause → mechanical fix →
postmortem.** Stop the bleeding first (revert / disable flag / roll back / take out of
rotation) *before* diagnosing. State the restore point out loud (which backup, what timestamp,
verified) before any recovery write. Diagnose only once it's stable.

**Why:** Recovery is where panic causes a *second* incident — a fix-forward under pressure, a
batch of changes you can't untangle, a "recovery succeeded" that didn't actually restore the
user-facing behavior. The instinct to fix fast is the trap. A panicked fix to incident #1 is
the usual cause of incident #2.

**How to apply:** The moment something's wrong in prod. One change at a time while recovering;
verify the *output* is healthy, not just that the recovery command exited 0; then fix the
*rule that let it happen* (a gate/canary/invariant), not just the instance. Full playbook in
`docs/claude/incident-response.md`. Pairs with [[feedback_production_mutation_preflight]],
[[feedback_enforce_safety_in_code_not_discipline]], [[feedback_verify_dont_infer_from_output]].
