# Cursor adapter: running the kit in Cursor

The kit is two layers. The **method** (the `docs/claude/*.md` playbooks) is plain markdown and
tool-neutral, so it works in any assistant. The **packaging** (auto-loading, subagents, memory)
was Claude Code-specific. This adapter ports the packaging to Cursor so the repo is dual-target.

## What maps to what

| Claude Code | Cursor | Notes |
|---|---|---|
| `CLAUDE.md` (auto-loaded context) | `.cursor/rules/*.mdc` with `alwaysApply: true` | Cursor doesn't read `CLAUDE.md`. The method ships as `operating-method.mdc`; project specifics as `project-context.mdc`. |
| The 5 method docs (`docs/claude/`) | same files, referenced from rules via `@path` | Tool-neutral. Kept as the single source of truth; rules stay thin and `@`-reference them. |
| `.claude/agents/*` (subagents w/ `tools:`) | Cursor **custom modes** or manual prompts | No direct equivalent. **Degradation:** Cursor can't structurally restrict a mode's tools, so "least-privilege tools" weakens from a wall to an instruction. Compensate by flagging risky ops for approval. |
| File-based memory + auto-recall | `.cursor/rules/lessons.mdc` (Agent-Requested) or Cursor **Memories** | The lessons port; the automatic recall mechanism differs. |
| `BOOTSTRAP-PROMPT.md` | `BOOTSTRAP-CURSOR.md` | Same flow, Cursor-worded; installs docs + `.cursor/rules`. |

## The rules shipped (`.cursor/rules/`)
- **`operating-method.mdc`**: `alwaysApply: true`. The persona + OOB + hard gates, inline, plus
  `@`-links to the full playbooks. The always-on core.
- **`project-context.mdc`**: `alwaysApply: true`. Template for this project's stack, conventions,
  locked decisions. The `CLAUDE.md`-equivalent; the bootstrap fills its `{{placeholders}}`.
- **`code-change.mdc`**: auto-attached (`globs` on source files). Points to the vigilance
  protocol + field notes when editing code. **Tune the globs** to your repo.
- **`ship-and-recover.mdc`**: Apply-Intelligently (`description`, no `globs`). Deploy discipline
  + the incident-response playbook; the Agent pulls it in for deploys/outages.

## Rule-type frontmatter (Cursor)
`.cursor/rules/*.mdc` files use three fields: `description`, `globs`, `alwaysApply`.
- **Always** → `alwaysApply: true`
- **Auto-attached** (file-matched) → `alwaysApply: false` + `globs`
- **Apply-intelligently** (Agent decides) → `alwaysApply: false` + `description`, no `globs`
- **Manual** (`@`-mention only) → `alwaysApply: false`, no `description`/`globs`
Plain `.md` in `.cursor/rules/` is ignored unless it's `.mdc` (or named `AGENTS.md`).

## Install
Use `BOOTSTRAP-CURSOR.md` (paste into the Cursor Agent, which self-clones and installs), or do it
by hand: copy `docs/claude/*` and `.cursor/rules/*` into your repo, fill `project-context.mdc`,
tune `code-change.mdc` globs, and fold `memory-seeds/*` into a `lessons.mdc` rule or Cursor
Memories.

## What you keep vs lose in Cursor
- **Keep (≈90%):** the whole method (persona, OOB, vigilance, field notes, incident response,
  the gates), applied automatically via always-on + auto-attached rules.
- **Lose / degrade:** structural tool-restriction on delegated agents (Cursor modes can't wall
  off tools the way Claude Code subagents can); automatic cross-session memory recall (becomes a
  rule). Both have the compensations noted above.

> Cursor's rules format and Memories feature evolve quickly. If something here doesn't match your
> Cursor version, the frontmatter fields (`description`/`globs`/`alwaysApply`) are the part to
> trust; check the current Cursor docs for the rest.
