---
name: feedback_gate_risky_work_behind_review
description: When driving a second model (CLI agent, IDE tool), the controller reads code and decides; the investigator proposes. Anything touching crash paths, data deletion, schema, auth, billing, or prod state stops for approval before code is written.
metadata:
  type: feedback
---

When more than one AI tool is in play, keep the roles clean: the **controller** holds the decision
and reviews by reading the actual code; the **investigator/implementer** explores and proposes in
its own context. **Gate:** any fix touching crash paths, data deletion, DB schema, auth, billing,
or production state requires controller approval *before* code is written: investigate → summarize
root cause + options → wait. Direct low-risk fixes with exact code; direct high-risk ones with
broad direction + explicit constraints, and review the output before it ships.

**Why:** A second model lacks your session context; letting its guess auto-become a shipped fix is
how wrong changes land. Splitting "explore" from "decide" keeps the controller's context clean for
judgment and puts a human-reviewable gate in front of anything irreversible. No model's output is a
finding until the controller has checked what produced it.

**How to apply:** Whenever a secondary model investigates or implements. Use the findings template
and cross-model second-opinion triggers in `docs/claude/multi-model-collaboration.md`. Pairs with
[[feedback_bug_reports_are_hypotheses]] and [[feedback_production_mutation_preflight]].
