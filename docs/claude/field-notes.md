# Field Notes: failure modes that burn real teams (project-agnostic)

A companion to `operating-principles.md` and `vigilance-protocol.md`. Principles tell you how to
work; these are the specific **traps**, each one having silently shipped a wrong result on a real
project before anyone noticed. They are written as *trap → why it's silent → the general rule*,
because the rule is the transferable part; the project that learned it is irrelevant.

Read this when you're about to deploy, migrate, gate a feature, or trust a build artifact: the
moments where the failure is invisible until production. Most of these pass every local test.

---

### 1. A committed migration is not a run migration.
**Trap:** a migration file lives in the repo, so everyone assumes it executed in prod. But the
deploy path only *syncs the schema* (adds/removes columns); it never runs the SQL in your
migration files. Any **data** migration (UPDATE/DELETE/INSERT) committed alongside it silently
never runs. The column exists; the data backfill didn't happen.
**Why it's silent:** schema diffs make the table *look* migrated. No error fires; the data step was
simply never invoked.
**Rule:** know whether your deploy *applies* migrations or merely *syncs* schema. If it syncs,
data migrations must be applied by hand (or by a one-shot script), and the commit message must
say so. Verify with a live count after deploy, not by trusting the file is present.

### 2. A disabled feature still has live schema and code paths.
**Trap:** a feature is flag-gated OFF, so it's treated as gone. But its columns, its accrual code,
and its orphaned rows from before the flag flipped are all still there, and a later "cleanup"
drops a column or purges rows that something still reads.
**Why it's silent:** the feature is "off," so its surface area feels like zero. It isn't.
**Rule:** gated-off ≠ removed. Expect nulls/zeros from its fields as *normal*, preserve its data
for audit, and don't delete its schema until the code that touches it is fully gone.

### 3. A doc/reality mismatch may be intentional.
**Trap:** public copy (pricing, a README claim, a config value) says X; the running system does Y.
The instinct is to "fix the bug" and make them match. But the divergence is deliberate (a
bootstrap subsidy, a staged rollout, a temporary override), and "fixing" it ships the wrong thing.
**Why it's silent:** a mismatch *looks* like an obvious bug, so it gets fixed reflexively.
**Rule:** before reconciling a doc-vs-reality gap, verify which one is intended. A mismatch is a
question, not a finding. (This is `verify-don't-infer` applied to documentation.)

### 4. The build cache served you a stale artifact.
**Trap:** you edit a file, rebuild, run, and your change has no effect. So you edit again,
differently, chasing a bug that's already fixed. The build system's up-to-date cache served
previously-compiled output and never recompiled your subproject/module.
**Why it's silent:** the build succeeds; the binary is just old. No error points at the cache.
**Rule:** when a change seems to have *no effect*, suspect the cache before you suspect your edit.
Force-clean the relevant subproject, rebuild, then re-test, before writing a second fix. (Pairs
with vigilance-protocol rule 7: don't pile on patches.)

### 5. Size/shape-gate your build output before you ship it.
**Trap:** the release artifact builds "successfully" but is far smaller than usual: a bundled
dependency (a large native lib, an asset pack) silently dropped out. It installs; it just doesn't
work. Discovered by users, not by CI.
**Why it's silent:** "build succeeded" and "build is correct" are different claims. A missing file
often isn't an error, just a smaller output.
**Rule:** add a mechanical gate on the *shape* of the output (artifact size ≥ a known floor, a
required file is present, a checksum matches), and fail the release loudly if it's off. An
artifact under threshold means missing files, not a leaner build.

### 6. Never advance a pointer ahead of the thing it points to.
**Trap:** the "current version" / "latest release" pointer (an env var, a redirect, a manifest) is
bumped *before* the artifact it references is actually live. Clients now request a version that
doesn't exist yet: broken downloads, "update available" loops, 404s.
**Why it's silent:** the bump itself succeeds instantly; the breakage only shows on the next client
fetch.
**Rule:** publish the artifact first, confirm it's live, *then* move the pointer. Ordering is the
safety property; encode it in the release runbook so it can't be reversed.

### 7. Know which "clean" commands destroy things that can't be rebuilt.
**Trap:** a routine `clean` / `prune` / `reset` wipes not just regenerable build output but an
input that has no source (a vendored binary, a generated-once asset, a local-only file), and the
next build silently produces a broken result instead of failing.
**Why it's silent:** the clean succeeds; the next build "works" but is subtly wrong/incomplete.
**Rule:** before running any destructive clean, know exactly what it deletes and whether every
deleted thing can be regenerated from source. If something can't, gate or ban that command and
document the safe alternative. (Make the safety mechanical: `operating-principles.md` A5.)

### 8. Don't assume a record carries an identity it doesn't store.
**Trap:** code keys off a field assuming the row "has" it (a conversation id on a job, a tenant id
on an event) when that field was never persisted on that table. The lookup quietly returns the
wrong rows or none.
**Why it's silent:** the field name *sounds* like it should be there; the query runs without error
and returns *something*.
**Rule:** verify a record actually stores a field before keying logic on it. Trace it to the write
site. (Pairs with vigilance-protocol rule 3: trace writes to readers.)

---

**How to use these:** they're patterns to *suspect*, not a checklist to recite. When you hit one of
these moments (deploying, migrating, gating a feature, trusting a build, reconciling a doc), scan
for the matching trap and run the one live check that would catch it. Add your own as the project
teaches you new ones; each new entry is a `feedback` memory with the same trap → why-silent → rule
shape.
