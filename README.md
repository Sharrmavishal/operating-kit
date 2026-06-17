# Claude Operating Kit

A portable, project-agnostic distillation of a disciplined "builder + strategic-vetting
partner" operating mode (the **persona**) and a critical-thinking / reframe-first
working style (the **OOB — out-of-box — approach**). It carries the *method*, not any
one project's domain context. Drop it into a new project and let that project's Claude
adopt it and grow its own specifics on top.

## What's in here

| File | What it is | Where it goes in the new project |
|---|---|---|
| `BOOTSTRAP-PROMPT.md` | Paste into the new project's **first** Claude session. Self-installs the rest. | (paste, don't copy) |
| `CLAUDE.md` | Generic project-context + operating-mode template with `{{PLACEHOLDERS}}`. | project root → `CLAUDE.md` |
| `docs/claude/operating-principles.md` | The full method: persona rules + OOB approach + the gates. | `docs/claude/` |
| `memory-seeds/` | ~8 generic, reusable "feedback" memories + an index. | the project's Claude memory dir |

## How to use it (two ways)

**Fast path (recommended):** open the new project in Claude, paste the contents of
`BOOTSTRAP-PROMPT.md`. It's self-contained from the GitHub URL — it first clones this kit
(`git clone https://github.com/Sharrmavishal/operating-kit ~/claude-operating-kit`), then
copies `operating-principles.md` in, writes the seed memories into that project's memory
dir, explores the codebase, and drafts a project-specific `CLAUDE.md` from the template —
then asks you to confirm the specifics. You only need the prompt block + the URL.

**Manual path:** copy `CLAUDE.md` + `docs/claude/operating-principles.md` into the new
repo, fill the `{{PLACEHOLDERS}}`, and copy `memory-seeds/*` into the project's memory
directory (the bootstrap prompt explains where that is).

## The one-line philosophy

Make the safety mechanical, not a thing you remember; treat every output as guilty until
you've checked what produced it; and when a plan can't ship, bring the reframe and the
options in the same breath as the blocker.
