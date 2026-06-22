---
name: feedback_search_before_you_name_it
description: Before labeling any named entity (record, field, ID, component) as new, a bug, a duplicate, or a regression, or filing/closing a tracker item on it, search the issue tracker (open AND closed) and your notes for its name, and check its created-at / git-blame before attributing a cause.
metadata:
  type: feedback
---

Before you conclude an entity is *new*, *a duplicate*, *missing*, *an orphan*, or *a
regression from X*, and before filing or closing a tracker item or acting irreversibly on
it, run a short context sweep on the entity's **name**. Four places: (1) the issue tracker,
**open AND closed** (it is often already tracked, so nest your finding in the existing item
instead of opening a duplicate); (2) your own notes/memory, reading the *body* and not just
the index; (3) the live system, not the doc's claim about it; (4) the decisions/locks doc,
so a "new framing" does not collide with a locked one. Then check **created-at / git-blame**
before you name a cause.

**Why:** Retrieval at the right moment beats recall. The failure mode is rarely lack of
knowledge; it is failing to retrieve what you already have before you conclude. Real case:
about to file "duplicate records, a regression from last week's migration." A 30-second
tracker search found the duplicate pairs already open as an issue from two months prior, and
the records' created-at predated the migration by weeks. The "regression" was hallucinated
causation. The search caught both the duplicate filing and the wrong cause, each of which
costs far more than the search.

**How to apply:** Auto-fires the moment you are about to (a) call something new, a bug, a
duplicate, missing, or a regression; (b) file, close, or comment on a tracker item; or
(c) mutate shared state. The test: "Before I call this new, broken, or mine, have I searched
the tracker and my notes for its name?" Distinct from its neighbors:
[[feedback_bug_reports_are_hypotheses]] is about *external* reports turning into a verdict;
this is about *your own* about-to-conclude label turning into a search.
[[feedback_check_premise_first]] ground-truths a brief's premise;
[[feedback_canonical_state_doc_not_memory]] verifies live state. Full discipline lives
alongside the vigilance protocol in `docs/claude/`.
