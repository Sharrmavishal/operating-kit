# Multi-Model Collaboration — controller + investigator pattern (project-agnostic)

A companion to `operating-principles.md`. Many people now drive more than one AI tool at once
— a primary assistant plus a second model (a CLI agent, an IDE bug-finder, a chat model with
no repo context). This file is the protocol for using them together **without** laundering one
model's guess into the other's authority.

The trap it prevents: *"the other tool said it's a bug, so I shipped the fix."* A second model
lacks your session context; you lack its independent angle. Both views must exist before
anything merges. (This is `vigilance-protocol.md` rules 5 and 9, made into a workflow.)

---

## Roles

**Controller (the primary assistant — usually the one reading this).**
Holds the decision. Reviews findings by **reading the actual code**, never reactively. Decides
risk level, approves or rejects, and directs the implementer with either exact code or
constrained direction. Owns the merge.

**Investigator / implementer (the secondary model — CLI agent, IDE tool, etc.).**
Does the heavy file exploration and option-surfacing in *its own* context (keeping the
controller's context clean for decisions). Proposes; does not unilaterally ship risky changes.

**Reviewer (a context-free model — a chat model with no repo access).**
Used deliberately as a second opinion on plans and architecture, *after* the controller has
formed its own view. Stress-tests; does not originate decisions.

---

## How the controller directs the implementer

- **Exact code** → for low-risk, well-scoped fixes: clear root cause, small diff, no judgment-
  call ambiguity. Spelling out the change eliminates interpretation errors.
- **Broad direction + explicit constraints** → for higher-risk fixes (crash paths, data
  deletion, production state, schema). Specify what to build **and what not to do, and why.**
  Review the output before it ships.

---

## Gate rule (hard)

**Any fix touching crash paths, data deletion, DB schema, auth, billing, or production state
requires controller approval before the implementer writes code.** The implementer must:

1. Investigate.
2. Summarize root cause + options (use the findings template below).
3. **Wait for approval.** Do not investigate-and-implement in the same step.

Small docs-only or comment-only changes are exempt.

---

## Findings template (what the investigator returns)

Loose prose hides the gaps. Require this structure:

- **Root cause:** one-line diagnosis.
- **Confidence:** high / medium / low — and *why*.
- **Code location:** file path + symbol or nearby line.
- **Files read:** the main files/logs actually inspected.
- **Relevant excerpt:** the smallest useful snippet.
- **Options:** A / B / C with trade-offs.
- **What I couldn't verify:** open gaps, missing logs, assumptions.
- **Risky?** yes/no — if yes, **stop after analysis and await approval.**

Rules: separate confirmed findings from hypotheses; say explicitly whether evidence is from
logs vs. code vs. inference; report multiple issues as distinct findings.

---

## When to pull in the context-free reviewer (second opinion)

A reviewer with no repo access has a precise capability **asymmetry** — use it for what it's good
at, distrust it on what it can't see:
- **Good at:** spotting an assumption you normalized over a long session, naming a risk you've
  stopped seeing, reframing a "build now vs. wait" call you're anchored on. No sunk-cost, no
  session momentum.
- **Bad at:** anything requiring repo truth. It will confidently *invent* a constraint, a column,
  or an API that doesn't exist — because it's reasoning from priors, not your code. Treat every
  concrete claim it makes as a hypothesis to check against the code (vigilance-protocol rule 5),
  never as fact.

So the rule is: pull it in **at decision boundaries, not for fact-finding** — the moments where
fresh framing beats local knowledge:
- a new plan has emerged from an analysis, before you commit build time to it;
- a significant architectural decision is made (routing, protocol, schema);
- you've crossed a scale/milestone threshold where old trade-offs deserve re-examination;
- right after a production incident, on the failure mode, before you commit to a fix.

**Why it works — and the failure it prevents:** the *value* is that the controller has already
formed a view, so the second opinion is a stress-test, not a crutch. The *failure* it guards
against is the opposite — adopting the reviewer's framing blind, which launds a context-free guess
into a decision. Both directions of that asymmetry matter: take its framing seriously, take its
facts skeptically.

---

## The one-line summary

> The controller reads code and decides. The investigator explores and proposes. The reviewer
> stress-tests after a view exists. No model's guess becomes a finding until the controller has
> checked what produced it.
