# Memory index (seed set: project-agnostic operating lessons)

These are portable method-memories. Add project-specific `feedback`/`project`/`reference`
memories alongside them as the project teaches you things.

- [Production-mutation pre-flight](feedback_production_mutation_preflight.md): Backup/Ownership/Blast-radius/Mechanism-on-copy/Memory before any irreversible shared-state write.
- [Enforce safety in code, not discipline](feedback_enforce_safety_in_code_not_discipline.md): A gate in your head gets skipped; encode it. Prod-write flags must not look like dry-runs.
- [Verify, don't infer from output](feedback_verify_dont_infer_from_output.md): A number isn't a finding until you've checked what produced it. Zero is a question.
- [Scratch-test before prod; dry-run only de-risks the executed path](feedback_scratch_test_before_prod.md): Run the mutating path on a copy, assert invariants, show the diff first.
- [Counts: check distinctness + distribution](feedback_counts_distinctness_and_distribution.md): Rows≠entities (COUNT DISTINCT); the mean hides skew. Most alarms are one of these.
- [Scope claims to evidence; no silent done](feedback_scope_claims_to_evidence.md): Visible review walk before "done/shipped/verified"; every claim backed by something you ran.
- [Surface the reframe + options with the blocker](feedback_surface_reframe_with_options.md): Never just "can't ship"; bring why + A/B/C + a recommendation in the same response.
- [Check the brief's premise first](feedback_check_premise_first.md): Ground-truth the assumption before executing; many briefs contradict the code/data.
- [Don't manufacture wait states](feedback_dont_manufacture_wait_states.md): Verify a gate is real before deferring; audit what's doable now.
- [Re-read & consolidate before patching](feedback_reread_and_consolidate_before_patching.md): Re-read the whole function before each edit; 3 edits to one file = STOP and write one consolidating fix. State the invariant.
- [Bug reports are hypotheses](feedback_bug_reports_are_hypotheses.md): A reported bug isn't a finding; read the code and write a verdict (real / false positive / investigate) before any fix.
- [Search before you name it](feedback_search_before_you_name_it.md): Before calling an entity new/a-bug/a-duplicate/a-regression or filing a tracker item, search the tracker (open+closed) and your notes for its name; check created-at before blaming a cause.
- [Gate risky work behind review](feedback_gate_risky_work_behind_review.md): Controller decides by reading code, investigator proposes; anything touching crash paths/data/schema/prod stops for approval first.
- [A committed migration is not a run migration](feedback_committed_migration_is_not_a_run_migration.md): Know if your deploy applies migrations or just syncs schema; data migrations often never auto-run. Verify with a live count.
- [Size-gate your build output](feedback_size_gate_your_build_output.md): "Build succeeded" ≠ "build is correct"; gate artifact size/shape so a missing bundled file fails loudly, not silently.
- [Incident response: stabilize first](feedback_incident_response_stabilize_first.md): Prod broke. Stabilize → name recovery point → recover → verify → root-cause → fix the rule. A panicked fix-forward causes incident #2.
- [Least-privilege subagent tools](feedback_least_privilege_subagent_tools.md): Scope each agent's tools to the minimum; a reviewer gets no Edit/Write. An absent tool is a wall; a prompt instruction is a request.
- [Canonical state doc, not memory](feedback_canonical_state_doc_not_memory.md): One source-of-truth doc for live state; verify against the running system; checkpoint before risky/large work.
- [Never commit secrets](feedback_never_commit_secrets.md): Env/.env (gitignored) + .env.example; scan the diff before push. A leaked secret must be rotated, not just deleted.

> Operational companions: `docs/claude/vigilance-protocol.md` (12-rule code-change checklist),
> `docs/claude/multi-model-collaboration.md` (controller + investigator pattern),
> `docs/claude/field-notes.md` (failure modes that ship silently), and
> `docs/claude/incident-response.md` (the recovery playbook, used when prod breaks).
