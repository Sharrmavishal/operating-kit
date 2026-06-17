# Bootstrap prompt — paste into the new project's FIRST Claude session

Self-contained: you only need this block and the GitHub URL — it clones the kit itself.
Copy everything in the fenced block below into Claude Code, running in the **new project's
root**. It fetches the portable operating method, installs it, and adapts it to *this*
codebase. It does not carry any other project's domain context — it builds the project
layer fresh from what it finds here.

```
Adopt a portable operating method for this project and install it from GitHub. Pause for my
confirmation before any write you're unsure about.

0. FETCH the kit. If ~/claude-operating-kit/operating-principles is not already present,
   get the kit:
     git clone https://github.com/Sharrmavishal/operating-kit ~/claude-operating-kit
   If that path already exists, instead run: git -C ~/claude-operating-kit pull --ff-only
   (If git clone isn't available, fall back to: degit / downloading the repo tarball from
   https://github.com/Sharrmavishal/operating-kit to ~/claude-operating-kit.)
   Confirm the files exist: ~/claude-operating-kit/docs/claude/operating-principles.md and
   ~/claude-operating-kit/memory-seeds/.

1. READ the method. Read ~/claude-operating-kit/docs/claude/operating-principles.md in
   full. From now on, operate by it: builder + strategic-vetting partner; permission
   discipline (read-only runs free, pause-and-propose before writes/deploys/irreversible
   shared-state mutations); the production-mutation pre-flight (Backup/Ownership/Blast-
   radius/Mechanism-on-a-copy/Memory) before any irreversible write; scratch-test
   destructive ops on a copy and show me the diff before prod; make safety mechanical
   (encode gates in code, don't rely on remembering them); a visible code-review + scope-
   vs-evidence walk before any "done/shipped/verified"; verify-don't-infer (an output is
   not a finding until you've checked what produced it).

2. INSTALL the docs. Copy ~/claude-operating-kit/docs/claude/operating-principles.md into
   this repo at docs/claude/operating-principles.md (create dirs as needed).

3. DRAFT this project's CLAUDE.md. Use ~/claude-operating-kit/CLAUDE.md as the template.
   Explore THIS codebase first (read the README, package/manifest files, the dir tree,
   the main entry points, the test setup, any deploy/CI config) and fill every
   {{PLACEHOLDER}} from what you actually find here — stack, what the project is, build/
   test/deploy commands, coding conventions, where secrets live. Leave "Locked decisions"
   and "Open work" empty for now. Do NOT invent specifics; if something's unclear, list it
   as a question for me rather than guessing. Write it to ./CLAUDE.md and show me a summary
   of what you filled in + your open questions.

4. SEED the memory. Write each file in ~/claude-operating-kit/memory-seeds/ into this
   project's Claude memory directory (the same directory your MEMORY.md lives in — it's
   referenced in your session context). Merge the seed MEMORY.md index lines into the
   existing MEMORY.md (don't overwrite anything already there). These are the portable
   method-memories; project-specific memories will accumulate alongside them over time.

5. CONFIRM. Summarize: what operating-principles you adopted, the CLAUDE.md you drafted
   (with open questions), and the memories you seeded. Then stop and wait for me to
   confirm/correct the project specifics before doing any real work.

Throughout: this is a fresh project. Carry the METHOD, not any other project's domain,
data, infra, or decisions. If you ever notice yourself about to apply a rule that was
specific to some other codebase, stop and re-derive it from this one.
```

---

## After bootstrap — how the project layer grows
- New durable lesson learned the hard way → write a `feedback` memory (with **Why** +
  **How to apply**) and add a one-line pointer to MEMORY.md.
- A settled architecture call → add it to CLAUDE.md "Locked decisions".
- Ongoing context not derivable from code/git → a `project` memory.
- The operating-principles file stays stable; only the project layer changes.
