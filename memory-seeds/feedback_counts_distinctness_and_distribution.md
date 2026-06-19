---
name: feedback_counts_distinctness_and_distribution
description: Rows are not entities (use COUNT DISTINCT); the mean hides skew (look at the distribution). Most alarming numbers are one of these.
metadata:
  type: feedback
---

Before a count drives a decision, check it for two illusions:
- **Distinctness:** rows ≠ entities. If a key can repeat (dup rows, multi-row-per-thing,
  no unique constraint), a raw `COUNT(*)` over-states — use `COUNT(DISTINCT ...)`.
- **Distribution:** a mean/average hides skew. A reasonable-looking average can be a few
  giants dragging up a long thin tail, with no actual item sitting anywhere near the mean.
  Look at the histogram/percentiles, not the average.

**Why:** Both routinely produce false alarms that would drive the wrong fix — an inflated
backlog number, a "typical" value that describes nothing real. **How to apply:** For any
headline number, ask "rows or entities?" and "mean or distribution?" before quoting it.
For composite scores, compute the distribution of the composite, not just its inputs.
Pairs with [[feedback_verify_dont_infer_from_output]].
