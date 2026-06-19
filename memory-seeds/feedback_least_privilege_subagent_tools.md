---
name: feedback_least_privilege_subagent_tools
description: Scope every subagent's tools to the minimum it needs: a reviewer gets no Edit/Write, a log-checker gets no deploy. Restrict by structure, not by hoping the prompt holds.
metadata:
  type: feedback
---

Give each subagent the **least privilege** it needs in its `tools:` frontmatter. A code
reviewer or log checker should have read-only tools (Read/Grep/Glob/Bash) and **no**
Edit/Write/deploy. A diagnostic agent should not be able to mutate state. Restrict the
*toolset*, don't just tell the agent "don't edit anything."

**Why:** A prompt instruction not to do something is a request; an absent tool is a wall. If a
read-only agent *can* write, one confused step (or a prompt-injection from page/log content it
reads) can mutate state it was never meant to touch. Structural limits make the unsafe action
impossible, not merely discouraged, the same "make safety mechanical" principle as in-code
gates.

**How to apply:** When defining any subagent, list only the tools its job requires, and
default to read-only unless it genuinely needs to write. Re-check on every new agent. Pairs
with [[feedback_enforce_safety_in_code_not_discipline]] and
[[feedback_gate_risky_work_behind_review]]. See `operating-principles.md` A5 and the templates
in `.claude/agents/`.
