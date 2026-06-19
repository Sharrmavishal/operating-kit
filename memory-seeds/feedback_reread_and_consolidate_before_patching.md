---
name: feedback_reread_and_consolidate_before_patching
description: Re-read the whole function before every edit; after 3 edits to one file, STOP and write one consolidating fix instead of a 4th patch. State the invariant the fix preserves.
metadata:
  type: feedback
---

Before changing a function you've already touched this session, **re-read it top to bottom** —
state added by an earlier edit silently invalidates the assumption you're about to make. If you
have edited the same file 3+ times, **stop**: list every change, find the *one* root cause they're
orbiting, and write a single consolidating fix instead of a 4th patch. For any correctness fix,
state the **invariant it preserves** in the commit message.

**Why:** Distilled from a session that shipped four incorrect fixes in a row to one handler, each
"correcting" the last. Reactive patching without holistic re-reading is the failure mode. If you
can't state the root cause (or the invariant) in one sentence, you don't understand the bug yet —
you're patching a symptom.

**How to apply:** Before any 2nd edit to the same function, before any state-machine change, and at
the 3-edits trigger. Full operational checklist (12 rules) in `docs/claude/vigilance-protocol.md`.
Pairs with [[feedback_bug_reports_are_hypotheses]] and [[feedback_verify_dont_infer_from_output]].
