# Show HN: Free crates.io API on SerenAI x402 Gateway

*AI agents can now query the Rust package registry—and combine it with code execution sandboxes and cron scheduling for automated dependency workflows.*

---

```
    ╔═══════════════════════════════════════════════════════════════╗
    ║                                                               ║
    ║      ██████╗██████╗  █████╗ ████████╗███████╗███████╗         ║
    ║     ██╔════╝██╔══██╗██╔══██╗╚══██╔══╝██╔════╝██╔════╝         ║
    ║     ██║     ██████╔╝███████║   ██║   █████╗  ███████╗         ║
    ║     ██║     ██╔══██╗██╔══██║   ██║   ██╔══╝  ╚════██║         ║
    ║     ╚██████╗██║  ██║██║  ██║   ██║   ███████╗███████║         ║
    ║      ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝   ╚═╝   ╚══════╝╚══════╝         ║
    ║                          .IO                                  ║
    ║                                                               ║
    ║         215K+ CRATES  •  213B+ DOWNLOADS  •  FREE             ║
    ║                                                               ║
    ╚═══════════════════════════════════════════════════════════════╝
```

## What's New

The official **crates.io API** is now available on the SerenAI x402 Gateway. It's **completely free**—no USDC payments required. AI agents can search Rust packages, check download stats, browse categories, and even manage crates (with your own API token).

### Endpoints Available

| Endpoint | Description |
|----------|-------------|
| `/crates/{name}` | Crate details, downloads, latest version |
| `/crates/{name}/versions` | All versions with download counts |
| `/crates?q={query}` | Search crates by keyword |
| `/crates?category={cat}` | Browse by category |
| `/summary` | Ecosystem stats (215K crates, 213B downloads) |
| `/categories` | List all categories |
| `/crates/{name}/{version}/yank` | Yank a version (auth required) |
| `/crates/{name}/owners` | Manage crate owners (auth required) |

## The Power Move: Rust Tooling Discovery + Execution

SerenAI hosts multiple developer-focused publishers that compose beautifully:

- **crates.io** — Find Rust packages (free)
- **Daytona** — Spin up code execution sandboxes ($0.001/sandbox)
- **SerenCron** — Schedule recurring jobs ($0.00001/exec)

Combine them for **automated Rust dependency workflows**.

### Example Prompt

```
Using SerenAI x402:

1. Query crates.io for the top 5 async runtime crates by downloads
2. For each crate, get the latest version and check its dependencies
3. Create a Daytona sandbox and test `cargo add {crate}` for each
4. Report which crates compile successfully and their dependency tree size
5. Schedule a weekly SerenCron job to re-check for version updates

I want to evaluate async runtimes for a new project and stay updated.
```

```
    ┌─────────────────────────────────────────────────────────────────┐
    │               RUST TOOLING DISCOVERY PIPELINE                   │
    ├─────────────────────────────────────────────────────────────────┤
    │                                                                 │
    │   crates.io (Free)         Daytona (Sandbox)                    │
    │   ┌─────────────┐          ┌─────────────────┐                  │
    │   │ Search pkgs │          │ cargo add test  │                  │
    │   │ Get versions│────────▶ │ Compile check   │                  │
    │   │ Dependencies│          │ Dep tree size   │                  │
    │   └─────────────┘          └─────────────────┘                  │
    │          │                          │                           │
    │          │    SerenCron (Schedule)  │                           │
    │          │    ┌─────────────────┐   │                           │
    │          └───▶│ Weekly updates  │◀──┘                           │
    │               │ Version alerts  │                               │
    │               └─────────────────┘                               │
    │                        │                                        │
    │                        ▼                                        │
    │               ┌─────────────────┐                               │
    │               │ Dependency      │                               │
    │               │ Health Report   │                               │
    │               └─────────────────┘                               │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘
```

### Real-World Use Cases

| Workflow | Publishers Used | Cost |
|----------|-----------------|------|
| Find web framework alternatives | crates.io | Free |
| Test if crate compiles on nightly | crates.io + Daytona | ~$0.001 |
| Weekly security audit of deps | crates.io + SerenCron | ~$0.00001/check |
| Compare serialization libs performance | crates.io + Daytona | ~$0.005 |

## Quick Start

### Via x402 MCP Server

```bash
# Search for async runtimes
mcp__x402__pay_for_query {
  "publisher_id": "43664d0d-3e8c-4bbb-836f-3b0ab5a1f5f7",
  "request": { "method": "GET", "path": "/crates?q=async+runtime&sort=downloads&per_page=5" }
}

# Get tokio details
mcp__x402__pay_for_query {
  "publisher_id": "43664d0d-3e8c-4bbb-836f-3b0ab5a1f5f7",
  "request": { "method": "GET", "path": "/crates/tokio" }
}

# Check tokio versions
mcp__x402__pay_for_query {
  "publisher_id": "43664d0d-3e8c-4bbb-836f-3b0ab5a1f5f7",
  "request": { "method": "GET", "path": "/crates/tokio/versions" }
}
```

### Via cURL

```bash
curl -X POST https://x402.serendb.com/api/proxy \
  -H "Content-Type: application/json" \
  -d '{
    "publisherId": "43664d0d-3e8c-4bbb-836f-3b0ab5a1f5f7",
    "request": { "method": "GET", "path": "/crates/serde" }
  }'
```

## Write Operations

crates.io supports authenticated write operations. Pass your crates.io API token:

```bash
# Yank a version (requires your crates.io token)
curl -X POST https://x402.serendb.com/api/proxy \
  -H "Content-Type: application/json" \
  -d '{
    "publisherId": "43664d0d-3e8c-4bbb-836f-3b0ab5a1f5f7",
    "request": {
      "method": "DELETE",
      "path": "/crates/my-crate/0.1.0/yank",
      "headers": { "Authorization": "cio_your_token_here" }
    }
  }'
```

The gateway forwards your token to crates.io—we don't store it.

## Why This Matters

Rust is eating infrastructure. From Cloudflare Workers to AWS Lambda, from Discord bots to blockchain nodes, Rust crates are the building blocks. AI agents that can:

1. **Discover** the right crate for a task
2. **Validate** it compiles and meets requirements
3. **Monitor** for updates and security issues

...are genuinely useful developer assistants, not just chatbots.

---

## About SerenAI

SerenAI is building payment infrastructure for the AI agent economy. Our x402 Gateway enables AI agents to autonomously pay for data and services using USDC micropayments on Base—no subscriptions, no API keys, no human intervention.

**Our Stack:** TypeScript, Rust, PostgreSQL, Cloudflare Workers, Base (Ethereum L2), Coinbase x402 Protocol

**Data Publishers:** Alpaca, CoinGecko, Firecrawl, Perplexity, OpenAI, Daytona, crates.io, Hacker News, and 35+ more

---

> **Ready to build?** Sign up at **[console.serendb.com/signup](https://console.serendb.com/signup)** with invite code **`serenai2025`**

*Questions? Email hello@serendb.com or join our [Discord](https://discord.gg/jseg7q4KS7).*

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
║              Payment Infrastructure for the AI Agent Economy                 ║
║                                                                              ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  TypeScript  •  Rust  •  Cloudflare Workers  •  Base  •  x402   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     DEVELOPER PUBLISHERS:                                                    ║
║     🦀 crates.io  •  🖥️ Daytona  •  ⏰ SerenCron  •  Free + Paid            ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: January 2, 2026*
*Tags: #ShowHN #SerenAI #x402 #CratesIO #Rust #Daytona #SerenCron #AIAgents #DeveloperTools #FreeTier*
