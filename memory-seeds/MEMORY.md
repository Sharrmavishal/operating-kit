# Memory index (seed set — project-agnostic operating lessons)

These are portable method-memories. Add project-specific `feedback`/`project`/`reference`
memories alongside them as the project teaches you things.

- [Production-mutation pre-flight](feedback_production_mutation_preflight.md) — Backup/Ownership/Blast-radius/Mechanism-on-copy/Memory before any irreversible shared-state write.
- [Enforce safety in code, not discipline](feedback_enforce_safety_in_code_not_discipline.md) — A gate in your head gets skipped; encode it. Prod-write flags must not look like dry-runs.
- [Verify, don't infer from output](feedback_verify_dont_infer_from_output.md) — A number isn't a finding until you've checked what produced it. Zero is a question.
- [Scratch-test before prod; dry-run only de-risks the executed path](feedback_scratch_test_before_prod.md) — Run the mutating path on a copy, assert invariants, show the diff first.
- [Counts: check distinctness + distribution](feedback_counts_distinctness_and_distribution.md) — Rows≠entities (COUNT DISTINCT); the mean hides skew. Most alarms are one of these.
- [Scope claims to evidence; no silent done](feedback_scope_claims_to_evidence.md) — Visible review walk before "done/shipped/verified"; every claim backed by something you ran.
- [Surface the reframe + options with the blocker](feedback_surface_reframe_with_options.md) — Never just "can't ship"; bring why + A/B/C + a recommendation in the same response.
- [Check the brief's premise first](feedback_check_premise_first.md) — Ground-truth the assumption before executing; many briefs contradict the code/data.
- [Don't manufacture wait states](feedback_dont_manufacture_wait_states.md) — Verify a gate is real before deferring; audit what's doable now.
- [Re-read & consolidate before patching](feedback_reread_and_consolidate_before_patching.md) — Re-read the whole function before each edit; 3 edits to one file = STOP and write one consolidating fix. State the invariant.
- [Bug reports are hypotheses](feedback_bug_reports_are_hypotheses.md) — A reported bug isn't a finding; read the code and write a verdict (real / false positive / investigate) before any fix.
- [Gate risky work behind review](feedback_gate_risky_work_behind_review.md) — Controller decides by reading code, investigator proposes; anything touching crash paths/data/schema/prod stops for approval first.

> Operational companions: `docs/claude/vigilance-protocol.md` (12-rule code-change checklist) and
> `docs/claude/multi-model-collaboration.md` (controller + investigator pattern).
