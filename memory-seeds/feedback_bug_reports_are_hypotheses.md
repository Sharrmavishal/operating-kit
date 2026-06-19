---
name: feedback_bug_reports_are_hypotheses
description: A bug report from any external reviewer (another AI tool, a linter, a teammate) is a hypothesis, not a finding. Read the surrounding code and write a verdict (real / false positive / needs investigation) BEFORE writing any fix.
metadata:
  type: feedback
---

When any external reviewer reports a bug, read the surrounding code **before** deciding it's real.
Ask what context the reporter lacked, what they couldn't see. Write a one-line verdict (**real /
false positive / needs investigation**) *before* writing a fix. Form your own independent view
first; a second opinion stress-tests that view, it doesn't replace it.

**Why:** External reviewers lack your session context, and out-of-context code routinely *looks*
buggy when it's fine: a "leak" in a `finally` that always runs, a "missing null guard" that exists
one line above, a "race" in an already-serialized path, "missing validation" a layer above enforces.
Shipping a fix for a non-bug introduces the regression the report only imagined. Equally: real bugs
do come in this way, so the rule is *read first, then decide*, never *accept* and never *dismiss*.

**How to apply:** On every externally-sourced bug report, before touching code. Pairs with
[[feedback_check_premise_first]], [[feedback_verify_dont_infer_from_output]], and
[[feedback_gate_risky_work_behind_review]]. Full discipline in `docs/claude/vigilance-protocol.md`.
