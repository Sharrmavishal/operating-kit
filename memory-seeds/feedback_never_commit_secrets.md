---
name: feedback_never_commit_secrets
description: Never commit secrets — keep them in env/.env (gitignored), maintain .env.example, and scan the diff before every push. A leaked secret is leaked even after you delete it.
metadata:
  type: feedback
---

Secrets (API keys, tokens, passwords, connection strings, private keys) never go in code,
logs, committed config, or a public surface. Keep them in environment variables / a gitignored
`.env`, maintain a `.env.example` with the *names* (not values), and **scan the diff before
every push** for anything secret-shaped.

**Why:** A secret pushed to a remote — especially a public one — is compromised the instant
it lands, even if you delete it next commit: it's in the history, in clones, in caches, in
mirrors. Rotation, not deletion, is the only real remedy after a leak. The cheap moment to
catch it is before the push.

**How to apply:** Before pushing, grep the staged diff for `key`/`token`/`secret`/`password`/
`BEGIN * PRIVATE` and obvious value patterns (`sk-`, `ghp_`, long hex/base64). Confirm `.env`
and credential files are gitignored. If one ever does land remote, **rotate it**, don't just
remove the commit. Pairs with [[feedback_enforce_safety_in_code_not_discipline]]; the
`code-review` agent checklist also covers it.
