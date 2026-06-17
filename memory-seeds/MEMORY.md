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
