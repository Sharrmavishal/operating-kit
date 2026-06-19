# Bootstrap prompt: Cursor

The Cursor counterpart of `BOOTSTRAP-PROMPT.md`. Self-contained from the GitHub URL. Open your
project in **Cursor**, open the **Agent** (a mode that can run terminal commands and edit
files), and paste the fenced block below as your first message. It carries the *method*, not
any other project's domain context.

```
Install a portable operating method into this project from GitHub, adapted for Cursor. Pause
for my confirmation before anything you're unsure about.

0. FETCH the kit:
     git clone https://github.com/Sharrmavishal/operating-kit ~/operating-kit
   (if the path exists, run: git -C ~/operating-kit pull --ff-only)
   Confirm ~/operating-kit/docs/claude/ and ~/operating-kit/.cursor/rules/ exist.

1. READ the method. Read ~/operating-kit/docs/claude/operating-principles.md in full and
   operate by it from now on: builder + strategic-vetting partner; pause-and-propose before
   writes/deploys/irreversible shared-state mutations; the production-mutation pre-flight
   (Backup/Ownership/Blast-radius/Mechanism-on-a-copy/Memory) before any irreversible write;
   scratch-test destructive ops on a copy and show the diff before prod; make safety
   mechanical; a visible code-review + scope-vs-evidence walk before any "done/shipped/
   verified"; verify-don't-infer.

2. INSTALL the method docs. Copy these into this repo (create dirs as needed):
   - ~/operating-kit/docs/claude/*.md  →  docs/claude/   (operating-principles, vigilance-
     protocol, multi-model-collaboration, field-notes, incident-response: the tool-neutral
     playbooks the rules below reference; the folder name is just where they live)

3. INSTALL the Cursor rules. Copy ~/operating-kit/.cursor/rules/*.mdc → .cursor/rules/ :
   - operating-method.mdc   (alwaysApply: true; the stable method + gates)
   - code-change.mdc        (auto-attached on code globs; TUNE the globs to this repo's
                             source dirs/extensions)
   - ship-and-recover.mdc   (pulled in for deploys/incidents)
   - project-context.mdc    (alwaysApply: true; fill it next)

4. FILL project-context.mdc. Explore THIS codebase (README, manifests, dir tree, entry points,
   test + deploy/CI config) and replace every {{PLACEHOLDER}} with what you actually find:
   stack, what the project is, build/test/deploy commands, conventions, where secrets live.
   Leave "Locked decisions" and "Open work" empty. Don't invent specifics; list anything
   unclear as a question for me.

5. CARRY OVER THE MEMORIES. The kit's ~/operating-kit/memory-seeds/ are reusable lessons.
   Cursor has no Claude-style memory dir, so fold them in one of two ways (ask me which):
   (a) condense them into a new .cursor/rules/lessons.mdc (description set, alwaysApply: false,
       so the Agent pulls them in when relevant), or
   (b) if this Cursor has the Memories feature enabled, add the high-value ones there.

6. NOTE WHAT DEGRADES IN CURSOR (tell me, don't silently drop):
   - Subagents with restricted tools (.claude/agents/) have no direct Cursor equivalent. The
     closest is Cursor custom modes, and Cursor can't structurally restrict an agent's tools,
     so "least-privilege tools" weakens from a wall to an instruction. Flag risky ops for my
     approval instead.
   - Auto memory-recall becomes rules (step 5).

7. CONFIRM. Summarize what you installed, the project-context.mdc you filled (with open
   questions), and how you handled the memories. Then STOP and wait for me to confirm the
   specifics before any real work.

Carry the METHOD, not any other project's domain, data, infra, or decisions.
```

See `docs/cursor-adapter.md` for the full Claude-Code ↔ Cursor mapping and what changes.
