---
name: feedback_scope_claims_to_evidence
description: Walk a visible review before any "done/shipped/verified"; every claim must be backed by something you actually ran or read.
metadata:
  type: feedback
---

Before emitting any closure word ("done", "shipped", "verified", "complete", "passes",
"ready"), walk the relevant checks **visibly in the response**, each answered:
- a concise code-review pass (correctness, error handling, injection/safety, idempotency,
  reversibility, consumer impact, performance, naming, secrets/leaks);
- **scope-vs-evidence:** every claim traces to something I ran or read, not inferred. If a
  step was skipped or a test failed, say so plainly.

**Why:** Silent claims of completion are the failure mode this prevents: "verified" that
was actually "looked plausible." Triggers like "empirically", "settled", "load-bearing"
are flags to re-check the claim against evidence. **How to apply:** Make the walk part of
the message, not an attestation line. Separate "the result was fine" from "the process was
sound." A good outcome by luck still failed the gate.
