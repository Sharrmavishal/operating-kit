# Vigilance Protocol — discipline for every non-trivial code change (project-agnostic)

A companion to `operating-principles.md`. Where the operating principles say *how to work*
and *how to think*, this file is the **operational checklist for touching code** — the
concrete moves that stop a clean intention from shipping a wrong fix.

It was distilled from a real failure: a session that shipped **four incorrect fixes in a
row** to the same handler, each "correcting" the previous one. The root failure was
**reactive patching without re-reading the whole function**, plus **deferring judgment to
an external reviewer** instead of forming an independent view. The rules below are
mandatory, not suggestions. Treat them as load-bearing invariants.

> Use this before any non-trivial code change — and *especially* before a 2nd edit to the
> same function in one session, any state-machine transition change, or accepting any
> externally-reported bug.

---

### 1. Re-read the entire function before every edit.
Before changing a function you have already edited this session, re-read it top to bottom —
not just the lines you are changing. State introduced by a previous edit silently
invalidates assumptions: a variable added two edits ago may now conflict with the check you
are about to add.

### 2. Enumerate failure causes before writing branching logic.
Any time you write a `try/catch`, a conditional recovery path, or handle a specific
DB/queue/HTTP error code (unique-constraint, conflict, type mismatch, 409, etc.), first
write down — in a comment or the commit message — the **distinct root causes** that can
trigger it. A single error code often has two unrelated causes (e.g. a unique-key violation
from a concurrent race *vs.* a genuine collision with someone else's row). Treating two
causes as one is the bug.

### 3. Trace every write to every reader before changing field semantics.
When you change what a field means, when it is written, or when it is cleared, grep for
**every reader** of that field before shipping. Grep the field name **and** its English
meaning (grep `notified` *and* grep "already sent" / "invite"). Readers in other files break
silently when the semantics shift under them.

### 4. Confident comments are a red flag.
When you are about to write a comment that starts "This field tracks X" or "We do Y because
Z" — stop. A comment is a permanent claim; if it is wrong, every future edit inherits the
lie. Verify the claim against at least one reader first. If you cannot cite a reader by
`file:line`, weaken the comment or delete it.

### 5. Bug reports are hypotheses, not findings.
When any external reviewer (another AI tool, a linter, a teammate, the user) reports a bug:
- Read the surrounding code **before** deciding the bug is real.
- Ask: did the reporter have the full context? What did they *not* see?
- Write a one-line verdict — **real / false positive / needs investigation** — *before*
  writing any fix.

Common false positives that look real out of context: a "leak" in a `finally` block that
always runs; a "missing null guard" that exists one line above; a "race" in a path that is
already serialized; "missing validation" that a layer above already enforces.

### 6. Tests that assert buggy behavior are worse than no tests.
When updating logic, re-read the existing test **assertions** — not just whether they pass.
A green test locks in whatever behavior it asserts. If a test asserts a call that was always
wrong, green is lying. Before editing a function ask: do the tests assert the behavior I
*want*, or the behavior that *was*? If the latter, fix the test first.

### 7. Patching-streak trigger: 3 edits to one file = STOP.
If you have edited the same file 3+ times in one session, do not make the next edit. Instead:
- Re-read the entire function section.
- List every change made to it this session.
- Identify the **one** root cause the changes are orbiting.
- Write one consolidating fix instead of a 4th patch.
- If you cannot state the root cause in one sentence, you do not understand the bug yet —
  stop coding and investigate.

### 8. Cross-identity writes are a stop-and-verify gate.
Any path that writes or reads a row identified by a field that is **not** the primary
identity key (a phone lookup when email is the key; an IP lookup when user-id is the key) is
a potential cross-identity leak. Before shipping, answer explicitly: *"If this non-primary
field collides with a different person's row, what does my code do?"* If the answer is
"emails them" or "updates their data," that **is** the bug — fix it now.

### 9. External review is a checkpoint, not a substitute for judgment.
When the user asks "should we have [other model] review this?", the answer is yes — **after**
you have formed your own independent view. Form the view first, then use the review to
stress-test it. Deferring judgment ("the tool said so") is how wrong fixes ship: the reviewer
lacks your session context, and you lack their independent angle. Both views must exist
before merging. (See `multi-model-collaboration.md`.)

### 10. State the invariant the fix preserves in the commit message.
For any correctness fix (not a typo or cosmetic refactor), the commit message must state the
**invariant** the fix preserves — e.g. "email is the authoritative identity key; phone
numbers get reused across people IRL." If you cannot state the invariant in one sentence, you
are patching a symptom, not the cause.

### 11. "Tests pass" is not "code is correct."
Green tests on a function you just rewrote prove only that the mocks you wrote match the logic
you wrote. They prove nothing about the real DB, real queue, real network, or real concurrent
callers. Before shipping a correctness fix, reason through at least one failure path *without*
the test mocks: "if I remove the mock and run this against the real system under load, which
of my assumptions breaks?"

### 12. Parallel/concurrent scenarios must be enumerated explicitly.
Whenever code does SELECT-then-write, an upsert, a GET-then-SET, or any read-modify-write,
explicitly write out: *"what happens if two requests hit this at the same moment?"* Do not
assume "the ORM handles it" or "the cache is atomic." Write the scenario down. If the code
does not handle it, either fix it or leave a `TODO(concurrency):` comment acknowledging the
gap.

---

**Why this exists:** born from a 4-fix-in-a-row regression on one handler. Reactive patching
without holistic re-reading, plus deference to external reviewers in place of independent
judgment, is what caused it. Encoding the discipline here makes it mechanical
(`operating-principles.md` A5) instead of something you have to remember mid-debug.
