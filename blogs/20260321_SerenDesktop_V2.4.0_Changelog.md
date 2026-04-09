# Seren Desktop v2.4.0 Changelog: Skill Card Redesign, Linux ARM64, and Critical Infrastructure Fixes

**Published**: March 21, 2026
**Reading Time**: 4 minutes
**Author**: Taariq Lewis, SerenAI

```
+================================================================================+
|                                                                                |
|     ____  _____ ____  _____ _   _   ____  _____ ____  _  _______ ___  ____     |
|    / ___|| ____|  _ \| ____| \ | | |  _ \| ____/ ___|| |/ /_   _/ _ \|  _ \   |
|    \___ \|  _| | |_) |  _| |  \| | | | | |  _| \___ \| ' /  | || | | | |_) |  |
|     ___) | |___|  _ <| |___| |\  | | |_| | |___ ___) | . \  | || |_| |  __/   |
|    |____/|_____|_| \_\_____|_| \_| |____/|_____|____/|_|\_\ |_| \___/|_|      |
|                                                                                |
|                        v 2 . 4 . 0   C H A N G E L O G                        |
|                             ~ March 21, 2026 ~                                 |
|                                                                                |
|    [Skills Redesign] [Linux ARM64] [Gateway Bridge] [Auth Fix] [Cache Clear]   |
|                                                                                |
+================================================================================+
```

## Overview

Seren Desktop v2.4.0 is a significant release that ships a redesigned skill management experience, expands Linux platform support to ARM64, and resolves a series of critical infrastructure bugs that affected publisher tool discovery, authentication timing, and update reliability. This release includes 9 commits across 15 files, touching frontend components, Rust backend commands, CI workflows, and build verification tooling.

## New Features

### Skill Card Redesign with Star Toggle, Trash Icon, and Browse Catalog

The skill management panel has been completely overhauled. The previous X-button removal flow is replaced by a star icon that serves as a clickable toggle for adding or removing a skill from the current thread. Active skills display a filled star; inactive skills show an outline. A dedicated trash icon now handles permanent skill deletion, complete with a confirmation dialog to prevent accidental removal. The top-level action menu has been split into two options: Browse Catalog opens an inline catalog view showing all available skills from the Seren marketplace with a one-click install button, while Skill Creator launches the custom skill authoring flow. Installing a skill from the catalog automatically adds it to the current thread and returns to the main skill list. Hidden skills are tracked in localStorage and excluded from the sidebar to keep the workspace clean.

### Linux ARM64 Release Target

Seren Desktop now builds for `aarch64-unknown-linux-gnu` in addition to the existing x86_64 target. This brings native ARM64 support to Linux users running on Raspberry Pi 5, AWS Graviton instances, Ampere workstations, and other ARM-based Linux hardware. The release workflow generates both `.deb` and `.AppImage` packages for the new architecture, and the updater manifest includes the ARM64 platform so existing users on compatible hardware can auto-update.

## Bug Fixes

### Clear Webview Cache Before Updater Relaunch

After an auto-update installed a new version, the webview cache could retain stale frontend assets from the previous release. Users would see the old UI until they manually cleared browser data or restarted multiple times. The updater now clears the webview cache before relaunching, ensuring the latest frontend is visible immediately after update.

### Route Gateway API Requests Through Rust Bridge

Gateway API requests were previously made directly from the frontend JavaScript layer. This bypassed the Rust backend's centralized authentication and error handling, leading to inconsistent behavior when tokens expired mid-session. All gateway API calls now route through the Tauri Rust bridge, which handles token refresh, retry logic, and structured error responses consistently across all request types.

### Await Auth Before Loading Threads

A race condition on cold start caused agents to spawn before authentication completed. `checkAuth()` was called without `await` in `App.tsx`, so the thread store loaded and spawned agent sessions before the API key was stored. This meant the Seren MCP server was never configured, and publisher tools like Gmail, Google Calendar, and Firecrawl were unavailable. The fix awaits authentication before any thread loading, with a defensive 3-second retry in `spawnSession` for edge cases where auth takes longer than expected. This resolves three previously mysterious symptoms: "No Seren publishers or agents are currently available," "Claude Code request failed" on cold start, and agents being unable to access Gmail and other OAuth publisher tools.

### Frontend Build Guard Verifies Against Current Commit

The previous build guard checked for frontend markers in the compiled binary, but Tauri compresses embedded assets, making `strings` searches unreliable. The guard now checks the `dist/` directory directly and verifies that the build output corresponds to the current git commit. This was added after v2.3.24 shipped with a stale February 15 frontend despite source changes — the build guard would have caught this and failed the CI pipeline before artifacts were uploaded.

### Preserve Existing Thread Skills on Upgrade

When the skill override system was introduced, existing threads lost their skills because the migration unconditionally returned an empty skill list. Now, threads without an explicit SQLite override fall back to project and global defaults, preserving the user's existing skill configuration. New threads start with an empty override so users manually curate which skills are active, but the upgrade path is non-destructive.

### Nested Button HTML Fix

The star toggle was implemented as a `<button>` nested inside the skill card's outer `<button>`, which is invalid HTML. Browsers silently collapsed the card content, making the entire skills list invisible. The inner element was changed to a `<span role="button">` with a keyboard event handler, restoring correct rendering across all browsers.

---

## About SerenAI

**SerenAI** builds agentic infrastructure for traders and developers. Seren Desktop is the open-source client that connects to the Seren Gateway ecosystem for AI chat, billing (SerenBucks), model access (Claude, GPT), publisher marketplace (Firecrawl, Perplexity, Gmail, Google Calendar, databases), and MCP server hosting for real-world actions.

**Download Seren Desktop:** [serendb.com](https://serendb.com)
**Documentation:** [docs.serendb.com](https://docs.serendb.com)
**Data Publishers:** Google, Nasdaq, Coinbase, Kraken, Alpaca, CoinGecko, Firecrawl, Perplexity, OpenAI, Moonshot AI, CoinMarketCap, and 110+ more

**Contact us:** [serendb.com](https://serendb.com) | [hello@serendb.com](mailto:hello@serendb.com)

```
+================================================================================+
|                                                                                |
|                           S  E  R  E  N  A  I                                  |
|                                                                                |
|              "Agents that trade. Infrastructure that pays."                     |
|                                                                                |
|         [SKILLS]---->[BACKTEST]---->[VALIDATE]---->[EXECUTE]                   |
|            |              |              |              |                       |
|            v              v              v              v                       |
|         install        dry-run       fail-closed     seren-cron                |
|         in 10s         first         always          scheduled                 |
|                                                                                |
|                 $$$   built for traders   $$$                                   |
|                                                                                |
|           ___          ___          ___          ___                            |
|          /   \  --->  /   \  --->  /   \  --->  /   \                           |
|         | dat |      | mod |      | str |      | exe |                          |
|          \___/        \___/        \___/        \___/                           |
|          data        models      strategy    execution                          |
|                                                                                |
|         serendb.com  |  hello@serendb.com  |  @seaborneai                      |
|                                                                                |
+================================================================================+
```
