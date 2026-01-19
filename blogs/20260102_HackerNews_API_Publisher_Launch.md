# Show HN: Free Hacker News API on SerenAI x402 Gateway

*AI agents can now query HN stories, comments, and users—and cross-reference with web scraping and AI search to build competitive intelligence tools.*

---

```
 ╔═════════════════════════════════════════════════════════════════════════════╗
 ║                                                                             ║
 ║  ██╗  ██╗ █████╗  ██████╗██╗  ██╗███████╗██████╗    ███╗   ██╗███████╗██╗   ██╗███████╗ ║
 ║  ██║  ██║██╔══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗   ████╗  ██║██╔════╝██║   ██║██╔════╝ ║
 ║  ███████║███████║██║     █████╔╝ █████╗  ██████╔╝   ██╔██╗ ██║█████╗  ██║ █ ██║███████╗ ║
 ║  ██╔══██║██╔══██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗   ██║╚██╗██║██╔══╝  ██║███╗██║╚════██║ ║
 ║  ██║  ██║██║  ██║╚██████╗██║  ██╗███████╗██║  ██║   ██║ ╚████║███████╗╚███╔███╔╝███████║ ║
 ║  ╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝   ╚═╝  ╚═══╝╚══════╝ ╚══╝╚══╝ ╚══════╝ ║
 ║                                                                             ║
 ║              STORIES  •  COMMENTS  •  USERS  •  FREE                        ║
 ║                                                                             ║
 ╚═════════════════════════════════════════════════════════════════════════════╝
```

## What's New

We've added the official **Hacker News API** to the SerenAI x402 publisher as [https://serendb.com/bestsellers/hacker-news-api](https://serendb.com/bestsellers/hacker-news-api). It's **completely free**. No payments required. AI agents can now access HN's real-time data through the same MCP interface they use for paid publishers.

### Endpoints Available

| Endpoint | Description |
|----------|-------------|
| `/topstories.json` | Top 500 stories |
| `/newstories.json` | Newest 500 stories |
| `/beststories.json` | Best stories |
| `/askstories.json` | Ask HN posts |
| `/showstories.json` | Show HN posts |
| `/jobstories.json` | Job postings |
| `/item/{id}.json` | Story, comment, or poll by ID |
| `/user/{username}.json` | User profile with karma and submissions |

## Build This: Show HN Competitor Tracker

SerenAI hosts [**SerenCron**](https://serendb.com/bestsellers/serencron) LLM-invoked cronjobs, [**Firecrawl**](https://serendb.com/bestsellers/firecrawl) (YC-backed web scraping) and [**Perplexity**](https://serendb.com/bestsellers/perplexity) (AI search). However, the emergent value-add for SerenAI is the composability of multiple publishers, and compute services, into a new service accessible by any LLM with MCP tooling. Combine SerenCron, Perplexity with Firecrawl and HN to build weekly, automated competitive intelligence.

### The Product: Automated Competitor Monitoring

Imagine you're building a developer tool. You want to know when competitors launch on Show HN, when their products get upvoted in comments, what features they're shipping, and how the HN community reacts.

This isn't just Show HN tracking. **Upvotes on links to competitor products anywhere on HN** are a strong signal. A competitor's blog post hitting the front page with 200 points? That's market intelligence you need to act on.

### Architecture

```
    ┌─────────────────────────────────────────────────────────────────┐
    │              SHOW HN COMPETITOR TRACKER                         │
    ├─────────────────────────────────────────────────────────────────┤
    │                                                                 │
    │   SerenCron ($0.00001/exec) ─── Triggers hourly/daily/weekly   │
    │                              │                                  │
    │                              ▼                                  │
    │   Hacker News API (Free)                                        │
    │   ├── /showstories.json    → Show HN launches                  │
    │   ├── /topstories.json     → Competitor mentions + upvotes     │
    │   └── /item/{id}.json      → Score, comments, URLs             │
    │                              │                                  │
    │                              ▼                                  │
    │   Firecrawl ($0.002/page)                                       │
    │   ├── Scrape competitor landing pages                          │
    │   └── Extract pricing, features, positioning                   │
    │                              │                                  │
    │                              ▼                                  │
    │   Perplexity ($0.005/query)                                     │
    │   ├── "Compare [competitor] vs [your product]"                 │
    │   └── "Summarize HN sentiment about [competitor]"              │
    │                              │                                  │
    │                              ▼                                  │
    │   OUTPUT: Slack alerts • Weekly reports • Feature matrices     │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘
```

### How It Works

1. **Define competitors** — Create a watchlist of domains and keywords
2. **Monitor Show HN** — Query `/showstories.json` for launches, check URLs against your list
3. **Track upvotes** — Scan `/topstories.json` for links to competitor domains (200 points on a competitor blog post = significant signal)
4. **Enrich with Firecrawl** — Scrape landing pages for pricing, features, positioning changes
5. **Analyze with Perplexity** — Get AI-powered competitive comparisons
6. **Automate with SerenCron** — Hourly checks, daily digests, weekly deep-dives

### Example Prompt

```
Using SerenAI x402:

My competitors are: RapidAPI (rapidapi.com), Hugging Face (huggingface.co)

1. Query Hacker News /topstories.json and /showstories.json
2. Find any stories linking to competitor domains or mentioning their names
3. For each match, get the story details: title, URL, score (upvotes), comment count
4. Use Firecrawl to scrape each competitor's landing page and changelog
5. Use Perplexity to answer: "What new features have these competitors launched recently? How does HN sentiment compare?"
6. Generate a competitive intelligence report with:
   - HN mentions ranked by upvotes
   - Feature updates from their sites
   - Community sentiment summary

I want this as a weekly digest.
```

### Cost Analysis

| Check Type | HN API | Firecrawl | Perplexity | SerenCron | Monthly |
|------------|--------|-----------|------------|-----------|---------|
| Show HN scan (hourly) | Free | — | — | $0.00001 | ~$0.01 |
| Top stories scan (hourly) | Free | — | — | $0.00001 | ~$0.01 |
| Landing page scrape | — | $0.002 | — | — | ~$0.12 |
| Competitive analysis (daily) | — | — | $0.005 | $0.00001 | ~$0.15 |
| **Total** | — | — | — | — | **~$0.20** |

### Ship It As

- **SaaS Dashboard**: $1/mo for founders tracking 5 competitors
- **Slack Bot**: Real-time alerts when competitors hit HN
- **Weekly Newsletter**: Curated competitive intel for your industry

## Quick Start

### Install the x402 MCP Server

GitHub: [github.com/serenorg/x402-mcp-server](https://github.com/serenorg/x402-mcp-server)

```bash
npx @serendb/x402-mcp-server
```

Or add to Claude Code:

```bash
claude mcp add x402 -- npx @serendb/x402-mcp-server
```

### Via x402 MCP Server

```json
// Get HN publisher details
mcp__x402__get_publisher_details { "publisher_id": "af2d7cbb-ca72-4031-9bbb-78c2cce6846e" }

# Query top stories
mcp__x402__pay_for_query {
  "publisher_id": "af2d7cbb-ca72-4031-9bbb-78c2cce6846e",
  "request": { "method": "GET", "path": "/topstories.json" }
}

# Get story details with score (upvotes)
mcp__x402__pay_for_query {
  "publisher_id": "af2d7cbb-ca72-4031-9bbb-78c2cce6846e",
  "request": { "method": "GET", "path": "/item/12345678.json" }
}
```

### Via cURL

```bash
curl -X POST https://x402.serendb.com/api/proxy \
  -H "Content-Type: application/json" \
  -d '{
    "publisherId": "af2d7cbb-ca72-4031-9bbb-78c2cce6846e",
    "request": { "method": "GET", "path": "/topstories.json" }
  }'
```

### Automate with SerenCron

Schedule weekly competitive scans via MCP:

```json
// Create weekly cron job (Monday 9 AM ET)
mcp__x402__pay_for_query {
  "publisher_id": "fdefd4a0-7a8b-4fb7-86d7-ec5b6ca69f19",
  "request": {
    "method": "POST",
    "path": "/api/v1/jobs",
    "body": {
      "name": "weekly-hn-competitor-scan",
      "url": "https://news.ycombinator.com/topstories.json",
      "method": "GET",
      "cron_expression": "0 9 * * 1",
      "timezone": "America/New_York"
    }
  }
}

// View stored results anytime
mcp__x402__pay_for_query {
  "publisher_id": "fdefd4a0-7a8b-4fb7-86d7-ec5b6ca69f19",
  "request": { "method": "GET", "path": "/api/v1/jobs/{job_id}/results" }
}
```

Results are stored and retrievable via SerenCron.

**Cost**: $0.00001/execution = ~$0.00004/month for weekly scans.

## Why Free?

Hacker News API is public—no API key required. We're not adding a toll to free data. Instead, we're making HN accessible through the same interface agents use for paid publishers, enabling cross-analysis workflows without switching contexts.

The value is in **composability** with both free and paid sources. An agent tracking competitors can pull HN discussions (free), scrape competitor sites with Firecrawl (paid), then get AI analysis from Perplexity (paid)—all through one protocol.

## About SerenAI

SerenAI is building payment infrastructure for the AI agent economy. Our x402 Gateway enables AI agents to autonomously pay for data and services using USDC micropayments on Base—no subscriptions, no API keys, no human intervention.

**Our Stack:** TypeScript, Rust, PostgreSQL, Cloudflare Workers, Base (Ethereum L2), Coinbase x402 Protocol

**Data Publishers:** Firecrawl, Perplexity, OpenAI, Exa, Google Trends, SEC Filings, Hacker News, crates.io, and 35+ more

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
║     COMPETITIVE INTEL STACK:                                                 ║
║     📰 Hacker News  •  🔥 Firecrawl  •  🔍 Perplexity  •  Free + Paid       ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: January 2, 2026*
*Tags: #ShowHN #SerenAI #x402 #HackerNews #Firecrawl #Perplexity #AIAgents #CompetitiveIntelligence #DeveloperTools #StartupTools*
