# Claude Code's Local Memory Is a Security Risk, and You Can Verify It Yourself

**Published**: April 9, 2026  
**Reading Time**: 6 minutes  
**Author**: Taariq Lewis, SerenAI

```text
+==================================================================================+
|                                                                                  |
|   C L A U D E   M E M O R Y   O N   D I S K                                      |
|                                                                                  |
|      ~/.claude/projects/*/memory/*.md                                            |
|      ~/.claude/projects/*.jsonl                                                  |
|                                                                                  |
|          plaintext notes        plaintext transcripts                            |
|               |                         |                                        |
|               +-----------+-------------+                                        |
|                           |                                                      |
|                     local filesystem                                             |
|                           |                                                      |
|                 backup tools / sync / malware / curious shell                    |
|                                                                                  |
+==================================================================================+
```

We learned yesterday that Claude Code leaves memory and session artifacts on the local filesystem under `~/.claude/projects/`, and on your machine those artifacts are easy to find and surprisingly rich.

Claude stores project memory under paths like `~/.claude/projects/*/memory/`. On my own machine I found multiple `memory/MEMORY.md` files and many sample files were mode `-rw-r--r--`, meaning world-readable to local users on the box. It contained operational preferences, release procedures, and references to secret-bearing environment variable names such as `ACCESS_KEY_ID` and `SECRET_ACCESS_KEY` for services that I had used for automation.

The less discussed part is the `.jsonl` session history. Across the broader `~/.claude/projects` tree, the largest retained transcripts on my machine were 30 MB to 41 MB of filesm each. Some artifacts date back to **January of this year**, which undercuts any claim that history is aggressively aged out by default. it appears that artifacts are not pruned. They can grow forever.

Here is what all of us using Claude for development should care about.

## What Is Actually Exposed?

First, the memory markdown files are plaintext. They can contain role assumptions, behavioral corrections, project conventions, internal links, and operating procedures.

Second, the `.jsonl` transcripts are often worse because they preserve prompts, tool results, current working directories, hook commands, and tool outputs. On my machine, a simple grep across `~/.claude/projects` found references to `API_KEY`, `SECRET`, `PASSWORD`, `PRIVATE_KEY`, `TOKEN`, `Authorization: Bearer`, and PEM/private-key markers. In one retained transcript, a bearer token was embedded directly in a shell command line captured in the log.

Third, there is no real application-layer access control around those files. The only protection is whatever your local filesystem already provides. That matters because same-user compromise is the normal shape of developer workstation attacks.

Fourth, there is no real audit trail at the file layer. If something reads, copies, modifies, or exfiltrates `~/.claude/projects/.../memory/*.md` or the `.jsonl` transcripts, there is no Claude-native ledger telling you when that happened.

Fifth, the portability story is weak. Claude's project directories are path-derived. Move machines, reinstall, or clone to a different path, and memory state becomes fragmented or stranded in local files.

## Verify It Yourself

You do not need to trust me. Run these commands on your own machine:

```bash
find ~/.claude/projects -path '*/memory/*.md' -type f | sed -n '1,20p'

find ~/.claude/projects -name '*.jsonl' | wc -l

find ~/.claude/projects -name '*.jsonl' -print0 | xargs -0 du -h | sort -hr | sed -n '1,20p'

rg -n --hidden -S '(API_KEY|SECRET|PASSWORD|PRIVATE_KEY|TOKEN|Authorization: Bearer)' \
  ~/.claude/projects -g '*.md' -g '*.jsonl' | sed -n '1,40p'

find ~/.claude/projects \( -name '*.jsonl' -o -path '*/memory/*.md' \) -type f \
  -exec stat -f '%Sm %N' -t '%Y-%m-%d %H:%M:%S' {} + | sort | sed -n '1,20p'
```

If you are security-minded, look for three things: plaintext memory, transcript files containing auth or tool output, and old artifacts lingering for months with no retention policy you explicitly configured.

```text
        .------------------------------------------------------------.
        |  QUICK THREAT MODEL                                        |
        |                                                            |
        |  User prompt      -> saved to .jsonl                       |
        |  Tool output      -> saved to .jsonl                       |
        |  Memory note      -> saved to .md                          |
        |                                                            |
        |  Same-user malware -> reads all of it                      |
        |  Backup/sync      -> copies all of it                      |
        |  No audit trail   -> you may never know                    |
        '------------------------------------------------------------'
```

## What Seren Desktop from SerenDB Does Better

The good news is that SerenDB's agent IDE, Seren Desktop, has a real interception layer for Claude's auto-memory markdown files. The app:

- Watches `~/.claude/projects/*/memory/`
- Parses Claude memory `.md` files
- Upserts them into a dedicated SerenDB table called `claude_agent_preferences`
- Creates an audit table called `claude_agent_preference_audit`
- Auto-provisions a separate project and database for Claude memory
- Uses authenticated HTTPS to the SerenDB SQL endpoint
- Starts only after the user is authenticated
- Migrates existing Claude memory files on startup if enabled
- Deletes the plaintext source `.md` file only after the SerenDB write succeeds

That is a materially better security posture than leaving scattered preference files as the system of record on disk.

There is another strong design decision here: Seren Desktop does not key memory by raw local path alone. It prefers a normalized git remote URL for project identity and falls back to a persisted UUID. That makes the canonical memory record portable across machines and clones.

There is also a credential hygiene improvement. The `seren-desktop` README and codebase are explicit that app credentials are stored with OS-level encryption rather than plain text config files.

## The Important Nuance

You just asked the right next question: Does this eliminate local plaintext Claude memories entirely?

Not completely.

Even with SerenDesktop, we still have a local copy of `MEMORY.md` from the database so Claude Code can consume it in its expected format. In other words, Seren Desktop makes SerenDB the canonical store, deletes the individual plaintext preference fragments after successful persistence, and reconstructs a single rendered file for compatibility. That is a real reduction in exposure, but it is not the same thing as "no plaintext ever exists locally."

Also, the code I reviewed targets the auto-memory markdown problem. The `.jsonl` transcript retention problem is broader.

Seren Desktop is a real security improvement for Claude memory management because it centralizes canonical state in authenticated SerenDB storage, adds migration and audit structure, reduces local file sprawl, and deletes intercepted source files after successful persistence. But if you want a complete local-artifact hardening story, transcript retention deserves the same treatment next.

```text
   Claude writes feedback.md
            |
            v
   [ Seren interceptor ]
            |
      INSERT into SerenDB
            |
      delete source .md
            |
   render canonical MEMORY.md
            |
         Claude reads
```

## Why This Matters Now

Agent memory is crossing from "developer convenience" into "persistent operator state." Once that happens, flat files stop being harmless and start becoming very dangerous.

Memory can reveal how you deploy, what you call important, what you recently fixed, what systems you use, and what your assistant has learned about your workflow. That is useful to red teamers, malware authors, insider threats, and anyone building tailored social engineering payloads.

Security professionals should be pushing AI tools toward database-backed, authenticated, auditable state now, before local artifact sprawl becomes normalized.

## About SerenAI

[SerenAI](https://serendb.com) is building infrastructure for the AI agent economy: Secure storage, agent-accessible APIs and agentic skills, hosted workflows, and the database layer behind persistent agent state.

Seren Desktop is the open source workspace https://github.com/serenorg/seren-desktop. SerenDB is the persistence layer. Together they let agents keep durable context without treating local plaintext files as the source of truth.

If you want Claude-compatible memory handling with a stronger security posture, the pattern is already visible in the repo: authenticate the user, provision dedicated storage, intercept local memory writes, persist them to a canonical database, migrate old artifacts, and stop relying on ad hoc flat files as the long-term memory system.

Start at [serendb.com](https://serendb.com).

```text
+==================================================================================+
|                                                                                  |
|   S E R E N A I                                                                  |
|                                                                                  |
|   Open source desktop workspace                                                   |
|   + authenticated agent memory storage                                            |
|   + database-backed state                                                         |
|   + structured auditability                                                       |
|                                                                                  |
|   serendb.com   docs.serendb.com   hello@serendb.com                              |
|                                                                                  |
+==================================================================================+
```

---

*Published: April 9, 2026*  
*Tags: #ClaudeCode #Security #SerenAI #SerenDB #AgentMemory #Privacy #HackerNews #AIAgents*
