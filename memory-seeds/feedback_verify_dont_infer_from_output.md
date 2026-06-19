---
name: feedback_verify_dont_infer_from_output
description: A number/output is not a finding until you've checked what produced it. Zero is a question, not a conclusion.
metadata:
  type: feedback
---

Treat every output as guilty until you've checked the mechanism behind it. A count, a
"0 results", a "succeeded": none are findings on their own. Test the *path that produces
the value* before you act on the value.

**Why:** This single rule reverses more wrong conclusions than any other. Examples of the
trap: reading a 200-byte response as "works" when it's an auth wall; "0 rows found" read
as "exhausted" when the source silently errored; a clean scratch result dismissed as an
"environment artifact" when it was faithfully reproducing a real prod condition.
**How to apply:** When a value drives a decision, add one probe that exercises the
generating mechanism (curl the actual body, check the error path, diff against a known
state). Don't let "cheap fix" round to "one-line fix" before the half you didn't test is
tested the way the half you did was. Pairs with
[[feedback_counts_distinctness_and_distribution]].
