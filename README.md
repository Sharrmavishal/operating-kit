# Claude Operating Kit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A portable, project-agnostic distillation of a disciplined "builder + strategic-vetting
partner" operating mode (the **persona**) and a critical-thinking / reframe-first
working style (the **OOB — out-of-box — approach**). It carries the *method*, not any
one project's domain context. Drop it into a new project and let that project's assistant
(Claude Code or Cursor) adopt it and grow its own specifics on top.

**Works with:** Claude Code (native) and Cursor (via the adapter — see [Using it with Cursor](#using-it-with-cursor)).

**Usage:** paste into your assistant and let it self-install — *“Clone https://github.com/Sharrmavishal/operating-kit and install it per its BOOTSTRAP-PROMPT.md (Claude Code) or BOOTSTRAP-CURSOR.md (Cursor), adapting to this repo.”*

## What's in here

| File | What it is | Where it goes in the new project |
|---|---|---|
| `BOOTSTRAP-PROMPT.md` | Paste into the new project's **first** Claude session. Self-installs the rest. | (paste, don't copy) |
| `CLAUDE.md` | Generic project-context + operating-mode template with `{{PLACEHOLDERS}}`. | project root → `CLAUDE.md` |
| `docs/claude/operating-principles.md` | The full method: persona rules + OOB approach + the gates. | `docs/claude/` |
| `docs/claude/vigilance-protocol.md` | The 12-rule operational checklist for touching code (the *how* behind the gates). | `docs/claude/` |
| `docs/claude/multi-model-collaboration.md` | Controller + investigator pattern for driving a second model safely (findings template + approval gate). | `docs/claude/` |
| `docs/claude/field-notes.md` | Catalog of failure modes that ship *silently* (migrations, gated features, build caches, release pointers): trap → why it's silent → the rule. | `docs/claude/` |
| `docs/claude/incident-response.md` | The recovery half — when prod breaks: stabilize → confirm recovery point → recover → verify → root-cause → mechanical fix → postmortem. | `docs/claude/` |
| `.claude/agents/` | Reusable subagent templates — `code-review`, `deploy`, `session-start`, `session-end`, `prod-logs` — with `{{placeholders}}`. | project root → `.claude/agents/` |
| `.cursor/rules/` | Cursor adapter — `operating-method` + `project-context` (always-on), `code-change` (auto-attached), `ship-and-recover` (`.mdc` rules). | project root → `.cursor/rules/` |
| `BOOTSTRAP-CURSOR.md` | Cursor counterpart of the bootstrap — paste into the Cursor Agent. | (paste, don't copy) |
| `docs/cursor-adapter.md` | The Claude Code ↔ Cursor mapping + what changes/degrades. | `docs/` |
| `memory-seeds/` | Generic, reusable "feedback" memories + an index. | the project's Claude memory dir |

## Quick start (bootstrap — recommended)

You don't clone or copy anything by hand. The `BOOTSTRAP-PROMPT.md` block is self-installing:
paste it once and Claude fetches the kit, installs it, and adapts it to *your* codebase.

1. **Open your target project in Claude Code** (the new repo's root must be the working dir).
2. **Open** [`BOOTSTRAP-PROMPT.md`](BOOTSTRAP-PROMPT.md) and **copy the entire fenced block** inside it.
3. **Paste that block as your first message** to Claude in the project.
4. **Let it run.** It will, in order:
   - clone this kit to `~/claude-operating-kit` (or `git pull` if already present);
   - read `docs/claude/operating-principles.md` and adopt the operating method;
   - install the method docs (`operating-principles`, `vigilance-protocol`,
     `multi-model-collaboration`, `field-notes`, `incident-response`) and the `.claude/agents/`
     templates into your repo;
   - explore your codebase and draft a project-specific `CLAUDE.md` from the template;
   - seed your project's Claude memory dir with the portable `memory-seeds/`.
5. **Confirm the specifics.** It pauses and shows you what it filled in + open questions — correct
   the stack/commands/decisions before it does any real work. Nothing else is needed but the
   prompt block and an internet connection.

> The only prerequisite is `git` (for the self-clone). If `git` isn't available, the prompt has a
> tarball/degit fallback baked in.

## Manual path (if you'd rather copy by hand)

Copy `CLAUDE.md` + everything under `docs/claude/` into the new repo, fill the `{{PLACEHOLDERS}}`,
copy `.claude/agents/*` into the project's `.claude/agents/`, and copy `memory-seeds/*` into the
project's Claude memory directory (the bootstrap prompt explains where that is). Then fill each
agent template's `{{placeholders}}` with the project's real commands and state doc.

## Using it with Cursor

The **method** is tool-neutral; the packaging ports via a small adapter. Paste
[`BOOTSTRAP-CURSOR.md`](BOOTSTRAP-CURSOR.md) into the Cursor Agent (it self-clones and installs),
or copy `docs/claude/*` + `.cursor/rules/*` in by hand. The kit ships ready-to-use
`.cursor/rules/*.mdc` (`operating-method` + `project-context` always-on, `code-change`
auto-attached, `ship-and-recover` on demand) that reference the same playbooks via `@path`.

You keep ~90% — persona, OOB, vigilance, field notes, incident response, the gates — applied
automatically. Two things degrade: Cursor can't *structurally* restrict a delegated agent's tools
(so least-privilege weakens from a wall to an instruction), and automatic memory recall becomes a
rule. Full mapping + caveats: [`docs/cursor-adapter.md`](docs/cursor-adapter.md).

## The one-line philosophy

Make the safety mechanical, not a thing you remember; treat every output as guilty until
you've checked what produced it; and when a plan can't ship, bring the reframe and the
options in the same breath as the blocker.

## Provenance & license

Distilled from real production work — each rule, trap, and gate earned by a failure that
shipped silently before it became a discipline. It's deliberately project-agnostic: it
carries the *transferable rule*, never the domain it came from, so it drops into any stack.

Licensed under the [MIT License](LICENSE) — reuse, adapt, and redistribute freely.
Adding to the kit? See [CONTRIBUTING.md](CONTRIBUTING.md) — keep every addition
project-agnostic and concrete (the two bars), in the `trap → why-silent → rule` shape.

