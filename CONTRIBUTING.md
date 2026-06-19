# Contributing

This kit carries **transferable method**, never project context. Every contribution must
clear two bars. Keep them both and the kit stays valuable; drop either and it rots into
vague advice or a data leak.

## The two bars (non-negotiable)

1. **Project-agnostic.** Nothing identifying any real project may ship: no product/company/
   competitor names, domains, hosts, IPs, secrets, customer data, schema/field names, or
   business specifics. Abstract the incident to the *rule*. "The project that learned it is
   irrelevant": only the lesson travels.
2. **Concrete, not generic.** A rule must be specific enough to *act on*: name the exact
   trap, the failure that's silent, and the one check that catches it. "Be careful with
   migrations" is noise; "a committed migration may only sync schema, data steps never run;
   verify with a live count" is signal. If it could appear in any generic blog post, sharpen
   it or cut it.

## Where things go

| You're adding… | Put it in… | Shape |
|---|---|---|
| A core principle (how to work / think) | `docs/claude/operating-principles.md` | A#/B# entry: rule + one-line *why* |
| An operational code-change rule | `docs/claude/vigilance-protocol.md` | numbered rule, mid-task actionable |
| A failure that ships *silently* | `docs/claude/field-notes.md` | **trap → why it's silent → the rule** |
| A reusable subagent | `.claude/agents/<name>.md` | frontmatter + `{{placeholders}}`, no real commands |
| A surfaced reminder of any of the above | `memory-seeds/<slug>.md` + index line | see format below |

Most lessons want **two** homes: the playbook entry (the full reasoning) **and** a one-fact
memory seed (so it surfaces contextually). Cross-link them.

## Formats

**Field note**: keep the three-part shape exactly:
```
### N. <the rule, as an imperative one-liner>.
**Trap:** what goes wrong and why it looks fine.
**Why it's silent:** why no error fires / why local tests pass.
**Rule:** the transferable rule + the one live check that catches it.
```

**Memory seed**: frontmatter + body with **Why** and **How to apply**, and `[[links]]` to
related seeds:
```
---
name: <slug>
description: <one line, used for recall relevance>
metadata:
  type: feedback   # or: project | reference
---
<the rule in 2-4 sentences>

**Why:** <the failure it prevents; the origin abstracted, never the project>
**How to apply:** <when to reach for it> Pairs with [[other-slug]].
```
Add a one-line pointer to `memory-seeds/MEMORY.md` (index = pointers, never content).

**Agent**: every project-specific value is a `{{PLACEHOLDER}}`. Never commit a real path,
command, host, endpoint, or ID. Keep the *shape* (e.g. the deploy agent's four gates)
regardless of stack. Scope `tools:` to least privilege: a review/diagnostic agent gets
read-only tools (no Edit/Write/deploy); an absent tool is a wall, a prompt instruction isn't.

## Self-check before you open a PR

- [ ] Re-read your diff as a stranger: does any line reveal *which* project taught you this?
- [ ] Run the leak sweep. It must return nothing:
  ```bash
  grep -rniE 'https?://|([0-9]{1,3}\.){3}[0-9]{1,3}|api[_-]?key|token|password|secret|<your-org-or-product-names>' --include='*.md' . \
    | grep -viE 'example\.com|github\.com/Sharrmavishal/operating-kit|no (hardcoded )?secrets|secrets/leaks'
  ```
- [ ] Every new specific is either universal (email-vs-phone identity, read-modify-write
      races) or a `{{placeholder}}`, never a real one.
- [ ] One logical addition per PR; cross-links updated (CLAUDE.md / README / operating-
      principles companions / MEMORY.md as relevant).
- [ ] It passes the two bars: a reader on a *different* stack can act on it, and a reader
      can't tell where it came from.

By contributing you agree your contribution is licensed under the repo's [MIT License](LICENSE).
