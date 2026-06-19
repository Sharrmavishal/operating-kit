# Incident Response — the recovery half (project-agnostic)

A companion to `operating-principles.md`. The rest of the kit is about *not breaking
production* — pre-flight, scratch-test, the backup gate, `field-notes.md`. This file is the
other half: **it broke, now what.** Recovery is where panic causes a *second* incident, so
the order below is the whole point — follow it in sequence, don't skip ahead to the fix.

> Read this the moment something is wrong in production. The instinct under pressure is to
> fix-forward fast; that instinct is the trap.

---

## The order that matters

### 1. Stabilize before you diagnose.
Stop the bleeding first. Revert the last change, disable the feature flag, roll back to the
previous release, or take the broken path out of rotation — **before** you understand the
root cause. A user-facing outage is not the time to debug; it's the time to get back to a
known-good state. Fix-forward under pressure is how you ship incident #2.

### 2. Confirm the recovery point — out loud — before you touch anything.
Know your restore point *and state it* (which backup, what timestamp, verified restorable)
before any recovery action. If you start mutating to recover without a confirmed point, you
can turn a recoverable incident into an unrecoverable one. (This is the production-mutation
pre-flight, applied mid-incident.)

### 3. Recover, one change at a time.
During recovery, make **one** change, then observe. Batching changes while firefighting means
you can't tell which one helped or hurt — and you can't cleanly undo. Slow is fast here.

### 4. Verify live — the output, not the exit code.
"Recovery script exited 0" is not "the system is healthy." Confirm the actual user-facing
behavior is restored: hit the real endpoint, check the real data, read the real logs. The
failure that caused the incident often hid behind a green status; don't let the recovery do
the same. (operating-principles A7/A9.)

### 5. *Now* root-cause — calmly, verify-don't-infer.
Only once it's stable, find the real cause. A number/log/output is not a finding until you've
checked what produced it (A7). Don't accept the first plausible story; the panic-era "obvious
cause" is wrong as often as not. Read the code, reproduce on a copy, pin the mechanism.

### 6. Ship the mechanical fix, not just the patch.
Fix the instance **and** the rule that let it happen, so it can't silently recur: an in-code
gate, a self-healing invariant, a loud canary, a size/shape check, a removed footgun. The
gate that lives in code holds; the one in your head gets skipped (A5). A fix that relies on
"we'll remember next time" hasn't closed the incident.

### 7. Write the one-paragraph postmortem.
Capture it as a `field-notes`/`feedback` entry: **what broke → why it was silent → the
mechanical guard now in place.** Blameless, short, and focused on the *systemic* gap, not the
person. The point is that the next person (or the next you) can't fall in the same hole.

---

## Rules under fire
- **Stabilize > diagnose > fix.** In that order, always.
- **One change at a time** while recovering.
- **State the recovery point before the first write** — no recovery action without a known-good
  restore point named.
- **A second incident almost always comes from a panicked fix to the first** — slow down at
  exactly the moment you want to speed up.
- **Verify the output is healthy**, never just that the recovery command succeeded.

## The one-line summary
> Get back to known-good first, with a recovery point named out loud; diagnose only once it's
> stable; then fix the *rule*, not just the instance, and write down why it was silent.
