# Introducing Your AI Audit of Seren Passwords

*Don't take our word for it. Paste this prompt into Claude, let it self-install the gateway, exercise every critical feature, and tell you — honestly — whether to migrate from 1Password or Bitwarden.*

---

## Why an AI audit, not a launch demo

**Seren Passwords is live at [passwords.serendb.com](https://passwords.serendb.com) — and it's the first password manager built to be tested *by* an AI agent.**

End-to-end encrypted on your device. Unreadable to the server. By design.

Your master password and item keys never leave your machine. Agents and CLIs resolve one credential field at a time through signed `seren-secrets://` references — without ever opening the whole vault. Vault admins can freeze any agent mid-session. Every resolve is hash-chained into an auditable log.

Three access paths, three scopes:

- **Vault invitation** — persistent team access
- **Live share** — one-off, email or identity, expires
- **Agent resolver** — field-level, signed, revocable

Every password manager claims "zero-knowledge." Fewer can be probed at the tool surface by an AI agent with no UI in the loop. That's the test we're inviting you to run.

## What this prompt does

The review prompt below is **self-bootstrapping**. The reviewer's Claude:

1. Reads [`docs.serendb.com/llms.txt`](https://docs.serendb.com/llms.txt) to load the docs index
2. Installs the Seren MCP gateway in one line — `claude mcp add --scope user --transport http seren https://mcp.serendb.com/mcp`
3. Authorizes through an OAuth 2.1 browser handshake on the first MCP call (no API key copy/paste)
4. Exercises every critical feature: CRUD, attachments, three sharing scopes, audit-log verification, import fidelity, lock/unlock, key rotation
5. Runs an adversarial round trying to break revocation, freeze, and access boundaries
6. Compares Seren Passwords against 1Password and Bitwarden on E2EE, daily UX, mobile, recovery, sharing model, AI integration, and maturity
7. Returns one of three verdicts: **MIGRATE NOW**, **MIGRATE A SUBSET**, or **WAIT** — with reasons

No fence-sitting allowed. The whole point of asking an AI to review is that it doesn't owe us social politeness.

## Before you start

You need a Claude with Claude Code or Claude Desktop installed. Nothing else. The prompt handles the rest, including a SerenAI account if you don't already have one — the OAuth flow on first MCP call signs you in or signs you up.

Run it against a **fresh, throwaway review vault** the first time. The prompt creates one for you and exercises everything inside it. Do not point this at your real 1Password or Bitwarden export until the prompt's Section 6 import test convinces you that TOTP secrets, attachments, and custom fields round-trip cleanly. That's the decisive migration test.

## The prompt

Paste this verbatim into Claude Code or Claude Desktop:

````
You are running an independent product review of Seren Passwords
(passwords.serendb.com), a new end-to-end encrypted, AI-first password
manager. I want an honest verdict at the end: should I export my 1Password
and Bitwarden vaults and migrate as a real test, or wait?

Be skeptical. If something is broken, missing, or worse than 1Password
or Bitwarden, say so. Do not soften.

## 0. Self-bootstrap

You do not need any Seren tools pre-installed. Do this first:

1. Read https://docs.serendb.com/llms.txt to load the docs index, then
   skim any pages you need for MCP install + Seren Passwords basics.

2. Install the Seren MCP gateway (hosted, OAuth-authenticated):

       claude mcp add --scope user --transport http seren https://mcp.serendb.com/mcp

   On your first MCP call, your browser will open and prompt you to
   authorize Claude against your SerenAI account. Approve it. The gateway
   mints a long-lived Bearer token and stores it for you — no API key
   copy/paste.

3. Confirm tools are live by calling `passwords_vaults_list`. If it errors
   with "tool not found" or "unauthorized", stop and tell me exactly what
   failed. Do not pretend to review what you couldn't reach.

## 1. Workspace setup

- `passwords_vault_create` — name "Claude Review — <today>"
- Confirm it appears in `passwords_vaults_list`
- Note the vault id for later

## 2. Core item lifecycle

Exercise the critical CRUD path against the review vault:

- `passwords_item_create` — a login (username, password, URL, TOTP, notes)
- `passwords_item_create` — a secure note
- `passwords_items_list` — confirm both appear
- `passwords_item_get` — fetch one back, confirm fields round-trip
- `passwords_item_update` — change a field, confirm it persists
- `passwords_item_duplicate` — confirm the copy is independent
- `passwords_item_move` — between vaults if you create a second
- `passwords_item_delete` → `passwords_item_restore` — confirm soft
  delete + restore
- `passwords_generate_password` — try several lengths/charsets, note
  output quality

Flag anything that fails, returns garbage, or behaves unlike the
1Password/Bitwarden equivalent.

## 3. Attachments

- `passwords_attachment_upload` — one text file, one binary
- `passwords_attachments_list` — confirm both
- `passwords_attachment_download` — confirm byte-for-byte round-trip
- `passwords_attachment_delete` — confirm removal

Note: size limits, type restrictions, UX rough edges.

## 4. Sharing — test all three scopes the product claims

a) Vault invitation (persistent team access):
   - `passwords_invitation_create` for a second email you control
   - `passwords_invitations_list` — confirm pending
   - `passwords_membership_grant` / `passwords_memberships_list` — verify

b) Live share (one-off, expiring):
   - `passwords_share_get` / outbound flow on a specific item
   - `passwords_shares_outbound_list` — confirm
   - `passwords_share_revoke` — confirm revocation is immediate

c) Agent resolver (field-level, signed):
   - `passwords_approval_request` for a specific field
   - `passwords_approval_approve` — confirm the field becomes resolvable
   - `passwords_agents_freeze` — freeze the agent mid-session, then try
     to resolve again, confirm the freeze takes effect *immediately*

For each path, answer: is the scope actually smaller than what 1Password
or Bitwarden gives a shared collection? Does revocation feel real?

## 5. Audit + verifiability

- `passwords_audit_events_list` — pull recent events
- `passwords_audit_verify` — confirm the hash chain verifies

Compare to 1Password Watchtower / Bitwarden event logs. More usable, less
usable, just different?

## 6. Import fidelity — the migration question

Do NOT use the user's real 1Password/Bitwarden export yet. Instead:

- Build a small synthetic 1Password .1pux (or .csv) and a Bitwarden .json
  export — 5–10 items each, mix of logins, secure notes, TOTP secrets,
  one attachment, one custom field, one folder/collection
- Run `passwords_vault_import` against each
- Round-trip every field: passwords, URLs, TOTP secrets (the secret
  itself, not just current codes), notes, attachments, custom fields,
  folder/collection mapping

This is the decisive test. If TOTP secrets, attachments, or custom fields
silently drop, the verdict is WAIT, regardless of how nice the rest is.

## 7. Lock / unlock / rotation

- `passwords_lock` then `passwords_unlock` — clean round-trip
- `passwords_vault_rotation_initiate` → `_complete` (or `_cancel`) —
  confirm rotation completes without data loss or audit gaps

## 8. Try to break it (adversarial round)

- Resolve a field after revoking the share — should fail
- Resolve a field after freezing the agent — should fail
- Try to modify an item from a member account that shouldn't have write
- Verify the audit log after each — do failed attempts show up?
- If `passwords_grant_status` exists, sanity-check what your current
  agent identity is actually allowed to do, and try one thing outside it

Report any case where the system let you do something it shouldn't, or
told you it did something it didn't.

## 9. Honest comparison vs 1Password + Bitwarden

Score on:

- E2EE guarantees (read the threat model if linked from llms.txt)
- Daily UX: item create, autofill, search — better, equal, or worse?
- Mobile + browser extension story: does it exist today, or CLI/MCP only?
- Recovery: what happens if I lose my master password? Is the recovery
  path actually safe?
- Sharing: is the 3-tier model genuinely tighter than 1Password
  collections / Bitwarden organizations, or just differently named?
- AI integration: is the agent-resolver model a real weekly-use
  differentiator, or a demo?
- Maturity: what's missing, half-built, or clearly v0.1?

## 10. Verdict — pick exactly one

- **MIGRATE NOW** — export 1Password/Bitwarden, use Seren Passwords as
  primary for 30 days. List the top 3 reasons.
- **MIGRATE A SUBSET** — move one category (team shared secrets, AI-agent
  credentials, etc.). Name the category and why.
- **WAIT** — keep 1Password/Bitwarden. List the top 3 blockers and what
  would change your mind.

No fence-sitting. Pick one.

## Cleanup

`passwords_vault_archive` (or equivalent) on the review vault. Confirm
no synthetic test data remains active.
````

## What the verdicts mean

- **MIGRATE NOW** is the bar we hope to clear, but we're not naive. It means import fidelity is clean, the three sharing scopes hold under adversarial pressure, the audit chain verifies, and the daily-use rough edges are tolerable. Top 3 reasons make the case crisp.
- **MIGRATE A SUBSET** is the realistic v1 outcome for most reviewers. Seren Passwords' agent-resolver model is genuinely new; the rest of the surface is competing with 5–10 years of incumbent polish. Moving just your AI-agent credentials or team shared secrets is a real bet without a full migration commitment.
- **WAIT** is the most useful verdict for us. The top 3 blockers list goes straight into the roadmap. If you reach this verdict, we'd rather hear it than not.

## Share what Claude told you

If you run the prompt, post the verdict — and the reasons. Tag [@SerenAI on LinkedIn](https://www.linkedin.com/company/serenai) or open an issue against [the seren-passwords repo](https://github.com/serendb/seren-passwords). Critical reviews get pinned. We're more interested in the WAIT lists than the MIGRATE NOW lists right now.

## One note on the bootstrap

The install line — `claude mcp add --scope user --transport http seren https://mcp.serendb.com/mcp` — is the only command the reviewer runs. There is no API key to paste, no `.env` to edit, no install script to `curl | bash`. The gateway uses OAuth 2.1 with PKCE; first MCP call opens a browser, you authorize, the gateway mints a long-lived Bearer token and stores it for you. Account creation happens inside the same flow if you don't already have one.

That's the whole product thesis in one command line: a password manager an AI can install, authenticate against, and review — without any human ever copying a secret into a chat.

🔐 [passwords.serendb.com](https://passwords.serendb.com)
📋 Prompt above. Paste, run, report back.
