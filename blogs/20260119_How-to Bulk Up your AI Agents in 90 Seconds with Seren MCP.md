# How to Bulk Up Your AI Agents in 90 Seconds with Seren MCP

*Give your AI assistant superpowers: access to 75+ data publishers, web scrapers, financial APIs, and more—all with a single command.*

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║     ███████╗███████╗██████╗ ███████╗███╗   ██╗ █████╗ ██╗                    ║
║     ██╔════╝██╔════╝██╔══██╗██╔════╝████╗  ██║██╔══██╗██║                    ║
║     ███████╗█████╗  ██████╔╝█████╗  ██╔██╗ ██║███████║██║                    ║
║     ╚════██║██╔══╝  ██╔══██╗██╔══╝  ██║╚██╗██║██╔══██║██║                    ║
║     ███████║███████╗██║  ██║███████╗██║ ╚████║██║  ██║██║                    ║
║     ╚══════╝╚══════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝                    ║
║                                                                              ║
║              PAY-PER-QUERY DATA MARKETPLACE FOR AI AGENTS                    ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

## The Problem: AI Agents Are Blind Without Data

Your AI assistant is powerful, but it's limited. It can't scrape websites, query financial databases, or access real-time information without complex API integrations. Every new data source means another API key, another SDK, another payment method to manage.

What if you could give your AI agent access to 75+ data publishers with a single installation?

## The Solution: Seren MCP Server

The **Seren MCP (Model Context Protocol) Server** connects your AI assistant directly to SerenAI's data marketplace. No local installations. No API key management. Just instant access to:

- **Web scraping** (Firecrawl, BrowserBase)
- **AI search** (Perplexity, Tavily)
- **Financial data** (stock prices, crypto rates)
- **Document processing** (PDFs, images, text extraction)
- **Package registries** (Crates.io, npm, PyPI)
- **And 70+ more publishers**

Pay only for what you use. No subscriptions. No monthly minimums.

---

## Quick Install: 90 Seconds or Less

### Universal Installer (Recommended)

One command auto-detects and configures all your installed MCP clients:

**macOS / Linux:**
```bash
curl -fsSL https://serendb.com/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://serendb.com/install.ps1 | iex
```

The installer automatically configures: **Claude Code, Claude Desktop, Cursor, Windsurf, OpenCode, Codex, and Gemini CLI**.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         SEREN MCP ARCHITECTURE                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌──────────────┐                          ┌────────────────┐             │
│   │              │    MCP Protocol          │                │             │
│   │  YOUR AI     │  ─────────────────────▶  │   SEREN MCP    │             │
│   │  ASSISTANT   │                          │   SERVER       │             │
│   │              │  ◀─────────────────────  │                │             │
│   └──────────────┘    Data + Results        └───────┬────────┘             │
│                                                     │                      │
│                                                     │ Queries + Payments   │
│                                                     ▼                      │
│                                             ┌────────────────┐             │
│                                             │  75+ DATA      │             │
│                                             │  PUBLISHERS    │             │
│                                             │                │             │
│                                             │  • Firecrawl   │             │
│                                             │  • Perplexity  │             │
│                                             │  • Crates.io   │             │
│                                             │  • And more... │             │
│                                             └────────────────┘             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Manual Setup by Platform

Prefer full control? Here's how to configure each platform manually.

### Claude Code (10 seconds)

```bash
claude mcp add --transport http -s user seren https://mcp.serendb.com/mcp
```

Verify with: `claude mcp list`

### Cursor (30 seconds)

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "seren": {
      "url": "https://mcp.serendb.com/mcp",
      "transport": "streamable-http"
    }
  }
}
```

Restart Cursor to apply.

### Windsurf (30 seconds)

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "seren": {
      "serverUrl": "https://mcp.serendb.com/mcp",
      "transport": "streamable-http"
    }
  }
}
```

Note: Windsurf uses `serverUrl` instead of `url`.

### Claude Desktop

Run the universal installer—Claude Desktop cannot self-configure:

```bash
curl -fsSL https://serendb.com/install.sh | bash
```

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         AUTHENTICATION OPTIONS                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│    ┌─────────────────────────┐    ┌─────────────────────────┐              │
│    │   OAUTH (DEFAULT)       │    │   X402 PAYMENTS         │              │
│    │                         │    │                         │              │
│    │  • Browser popup        │    │  • On-chain signatures  │              │
│    │  • Sign in once         │    │  • Pay per request      │              │
│    │  • Auto-refresh tokens  │    │  • No account needed    │              │
│    │                         │    │                         │              │
│    │  ZERO CONFIGURATION     │    │  FULLY PERMISSIONLESS   │              │
│    └─────────────────────────┘    └─────────────────────────┘              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## First Use: Authentication

On first use, Seren prompts for OAuth authentication:

1. A browser window opens to `serendb.com`
2. Sign in or create a free account
3. Authorize the connection
4. Return to your AI assistant

No API keys to copy. Authentication is automatic.

---

## Real-World Example: Crates.io Publisher

Let's see Seren MCP in action with a developer-focused use case. The **Crates.io Publisher** lets your AI agent query the Rust package registry directly.

**Scenario:** You need to find the most downloaded Rust crates for a dependency analysis.

**Without Seren:** You'd manually browse crates.io, copy-paste data, or write a custom API client.

**With Seren:** Just ask your AI assistant:

```
"Find the top 10 most downloaded crates on crates.io"
```

Your AI agent calls the Crates.io publisher via `execute_paid_api`:

```json
{
  "publisher": "crates-io",
  "method": "GET",
  "path": "/crates?sort=downloads&per_page=10",
  "headers": {"User-Agent": "my-agent (contact@example.com)"}
}
```

**Result:** Live data from crates.io (as of January 19, 2026):

| Crate       | Downloads    | Description                                |
| ----------- | ------------ | ------------------------------------------ |
| syn         | 1.30B        | Parser for Rust source code                |
| hashbrown   | 1.10B        | Google's SwissTable hash map port          |
| bitflags    | 978M         | Macro to generate bitflag structures       |
| proc-macro2 | 896M         | Substitute for compiler's proc_macro API   |
| libc        | 877M         | Raw FFI bindings to platform libraries     |
| quote       | 877M         | Quasi-quoting macro quote!(...)            |
| base64      | 875M         | Base64 encoding/decoding                   |
| getrandom   | 861M         | Cross-platform random data from system     |
| rand_core   | 852M         | Core RNG traits and tools                  |
| rand        | 835M         | Random number generators                   |

**Cost:** $0.00 (free tier). **Execution time:** 97ms. **No API key required.**

---

## Available Tools After Installation

| Tool | What It Does |
|------|--------------|
| `list_agent_publishers` | Browse 75+ data publishers |
| `execute_paid_query` | Query publisher databases (SQL) |
| `execute_paid_api` | Call publisher HTTP APIs |
| `get_wallet_status` | Check your prepaid balance |

**Try it now:** Ask your AI assistant to *"list available Seren publishers"*

---

## Why Seren MCP?

**No vendor lock-in.** Access multiple competing services through one interface. Switch from Firecrawl to BrowserBase without changing your integration.

**Pay per query.** No subscriptions. Use Firecrawl once? Pay $0.01. Use it 1000 times? Pay $10. Use nothing? Pay nothing.

**AI-native design.** Built for agents, not humans clicking buttons. Structured responses, predictable pricing, instant settlement.

**Instant access.** Install in 90 seconds, start querying immediately. No sales calls, no enterprise contracts, no waiting for API key approvals.

**Developer-first publishers.** Access Crates.io, npm, PyPI, and other package registries your AI needs to help you code faster.

---

## Get Started

1. Run the installer: `curl -fsSL https://serendb.com/install.sh | bash`
2. Ask your AI: *"List Seren publishers for web scraping"*
3. Execute a query: *"Use Firecrawl to scrape https://example.com"*
4. Check balance: *"What's my Seren wallet balance?"*

Your AI agent just got 75x more capable.

---

## Resources

- **Full API Documentation:** [docs.serendb.com](https://docs.serendb.com)
- **MCP Installation Guide:** [docs.serendb.com/guides/mcp-install](https://docs.serendb.com/guides/mcp-install.html)
- **Console (Sign Up):** [console.serendb.com](https://console.serendb.com)
- **Discord Community:** [discord.gg/jseg7q4KS7](https://discord.gg/jseg7q4KS7)

---

```text
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║     ███████╗███████╗██████╗ ███████╗███╗   ██╗ █████╗ ██╗                    ║
║     ██╔════╝██╔════╝██╔══██╗██╔════╝████╗  ██║██╔══██╗██║                    ║
║     ███████╗█████╗  ██████╔╝█████╗  ██╔██╗ ██║███████║██║                    ║
║     ╚════██║██╔══╝  ██╔══██╗██╔══╝  ██║╚██╗██║██╔══██║██║                    ║
║     ███████║███████╗██║  ██║███████╗██║ ╚████║██║  ██║██║                    ║
║     ╚══════╝╚══════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝                    ║
║                                                                              ║
║              Pay-Per-Query Data Marketplace for AI Agents                    ║
║                                                                              ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  docs.serendb.com  •  console.serendb.com  •  mcp.serendb.com   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: January 19, 2026*
*Tags: #SerenAI #MCP #AIAgents #ClaudeCode #Cursor #Windsurf #DataMarketplace #Micropayments #Crates #RustLang*
