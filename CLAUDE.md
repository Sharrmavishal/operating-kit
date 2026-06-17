# CLAUDE.md — {{PROJECT_NAME}} Project Context

Read before every task. Operating method (how I work + how I think) is in
`docs/claude/operating-principles.md` — that file is stable across projects; this file
holds **this** project's specifics and grows over time.

---

## What is {{PROJECT_NAME}}
{{One-paragraph description: what it does, who it's for, current phase/state.
Replace this whole block.}}

**Live state:** {{prod URLs / environments / current milestone}}.

---

## Tech stack
{{Languages, frameworks, DB, infra, key services. Note where secrets live and that they
are never committed. Note any non-obvious build/deploy step (e.g. "run the build locally
before pushing — CI lint blocks deploys").}}

---

## Operating mode (pointer)
Builder + strategic-vetting partner. Full rules in `docs/claude/operating-principles.md`.
The load-bearing gates, inline so they're never missed:

- **Permission discipline.** Read-only/diagnostic runs proceed freely. **Pause and
  propose** before code edits to shared paths, DB writes, deploys, issue creation,
  canonical-doc edits, and any irreversible shared-state mutation. Pre-authorized
  sequences ("ship it", an explicit ordered plan) proceed.
- **Production-mutation pre-flight (hard gate).** Before any irreversible shared-state
  write, answer in the response: Backup / Ownership / Blast-radius / Mechanism-tested-on-
  a-copy / Memory-check. (operating-principles A3.)
- **Scratch-test destructive ops** on a copy and show the diff before prod. (A4.)
- **Make safety mechanical** — encode gates in code, don't rely on remembering them. (A5.)
- **Milestone gate.** Walk a visible code-review + scope-vs-evidence pass before any
  "done/shipped/verified". (A6.)
- **Verify-don't-infer.** An output is not a finding until you've checked what produced
  it. (A7.)

---

## Coding standards
{{Project conventions. Examples to adapt:}}
- Error handling norms (e.g. background jobs log-and-continue, never crash on bad data).
- No hardcoded secrets; maintain `.env.example`.
- Destructive DB ops only via the approved/safe path (name it here once one exists).
- One logical change per commit; meaningful messages.
- Update CLAUDE.md + CHANGELOG + memory on every significant change.

---

## Pre-ship verification (project gates)
{{Add gates as you learn the failure modes. Each gate = a live check that returns a count
with an explicit pass condition. Seed examples:}}
- **Post-change gate:** every data-transform/serialization/config change runs a live
  curl/query/grep returning a count before being called done.
- **Consumer audit:** after an API/shape change, hit each consuming route, not just the
  endpoint that changed.
- **Calibration:** verify any hardcoded threshold/enum against a live distribution before
  shipping, not after.

---

## Locked architectural decisions — do not relitigate
{{Index of decisions that are settled. Add as they're made; link to a one-pager for the
bulky ones. Empty to start.}}
- _(none yet)_

---

## Open work
{{Key durable threads / issues. Keep short; point to the issue tracker for the full list.}}
- _(none yet)_

---

## Quick reference
{{Common commands: build, test, run the pipeline, inspect the DB, deploy. Fill in.}}
```bash
# build / test / run
# deploy
# inspect data
```
