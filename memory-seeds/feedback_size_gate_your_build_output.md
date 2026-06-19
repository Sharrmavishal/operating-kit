---
name: feedback_size_gate_your_build_output
description: "Build succeeded" is not "build is correct." Add a mechanical gate on the shape of the release artifact (size ≥ a known floor, required files present, checksum matches) and fail the release loudly if it's off. An artifact under threshold means missing files, not a leaner build.
metadata:
  type: feedback
---

Gate the *shape* of every release artifact before shipping it: size ≥ a known floor, required files
present, checksum matches. A successful build that is far smaller than usual has silently dropped a
bundled dependency (a native lib, an asset pack). It installs and runs but doesn't work. Fail the
release loudly when the shape is off; don't let a green build imply a correct one.

**Why:** "build succeeded" and "build produces a working artifact" are different claims. A missing
file is usually not a compile error, just a smaller output, so it sails past CI and is found by
users instead of by you. A mechanical size/shape gate turns a silent omission into a loud stop.

**How to apply:** add the gate to the release runbook/script, not to human memory. Encode it
(`operating-principles.md` A5). Pairs with [[feedback_scope_claims_to_evidence]] and
[[feedback_enforce_safety_in_code_not_discipline]]. More release traps in
`docs/claude/field-notes.md`.
