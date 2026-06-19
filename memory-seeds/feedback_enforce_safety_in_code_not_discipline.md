---
name: feedback_enforce_safety_in_code_not_discipline
description: A safety gate that lives in a human's head gets skipped; encode it. Prod-write flags must not look like dry-runs.
metadata:
  type: feedback
---

When you catch a near-miss, the fix is **mechanical, not "be more careful."** The gate
that lives in code holds; the gate that lives in your head gets skipped, usually on the
command you thought was harmless.

**Why:** Real lapses happen on autopilot (e.g. running `--apply` in a command intended as
a dry-run, skipping a backup-confirm). "Resolve to remember" fails the next time attention
dips. **How to apply:**
- Encode the check: in-script backup confirmation that refuses `--apply` without a recent
  restore point and prints it back; self-healing invariants; a canary that fails loud.
- A destructive command must not be one keystroke from a safe one. For non-prod runs,
  override the target explicitly and never include the write flag.
- After fixing the instance, fix the *rule that let it in*, same as preferring a typed
  guard over a comment. Pairs with [[feedback_production_mutation_preflight]].
