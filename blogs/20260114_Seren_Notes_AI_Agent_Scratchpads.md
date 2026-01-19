# Introducing Seren Notes: API-First Scratchpads for AI Agents

*Your LLM needs a place to think. Seren Notes gives AI agents a lightweight, pay-per-call notepad they can read, write, and visualize—without permission prompts or subscription overhead.*

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║   ███████╗███████╗██████╗ ███████╗███╗   ██╗                                ║
║   ██╔════╝██╔════╝██╔══██╗██╔════╝████╗  ██║                                ║
║   ███████╗█████╗  ██████╔╝█████╗  ██╔██╗ ██║     ███╗   ██╗ ██████╗ ████████╗███████╗███████╗
║   ╚════██║██╔══╝  ██╔══██╗██╔══╝  ██║╚██╗██║     ████╗  ██║██╔═══██╗╚══██╔══╝██╔════╝██╔════╝
║   ███████║███████╗██║  ██║███████╗██║ ╚████║     ██╔██╗ ██║██║   ██║   ██║   █████╗  ███████╗
║   ╚══════╝╚══════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝     ██║╚██╗██║██║   ██║   ██║   ██╔══╝  ╚════██║
║                                                  ██║ ╚████║╚██████╔╝   ██║   ███████╗███████║
║                                                  ╚═╝  ╚═══╝ ╚═════╝    ╚═╝   ╚══════╝╚══════╝
║                                                                              ║
║            ┌────────────────────────────────────────────────────┐            ║
║            │   🤖 CREATE  •  📖 READ  •  ✏️ UPDATE  •  🗑️ DELETE  │            ║
║            │         API-First Scratchpads for LLMs             │            ║
║            └────────────────────────────────────────────────────┘            ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

## The Problem: LLMs Need Persistent Memory

When you're working with an AI agent on [SerenAI](https://serendb.com), something awkward happens. Your agent queries databases, pulls from APIs, analyzes documents—then has nowhere to put the results where you can actually see them.

Sure, the agent can dump text into chat. But what if it needs to:
- Save intermediate calculations for a multi-step analysis?
- Create a table comparing data from three different publishers?
- Draw a flowchart showing how payment routing works?
- Keep running notes across multiple sessions?

Today, agents either clutter your conversation with walls of text or lose their work entirely when the session ends. **Seren Notes fixes this.**

## What Is Seren Notes?

Seren Notes is a lightweight, API-first note-taking service built specifically for AI agents. It's hosted at [notes.serendb.com](https://notes.serendb.com) and accessible via [seren-MCP](https://serendb.com/mcp), the same protocol your agents already use for paid API calls on SerenAI.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     HOW SEREN NOTES WORKS                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│     ┌─────────┐         ┌──────────────────┐         ┌─────────────┐       │
│     │   LLM   │  CRUD   │   Seren Notes    │  VIEW   │    USER     │       │
│     │  Agent  │ ──────► │   Publisher API  │ ◄────── │  Dashboard  │       │
│     └─────────┘         └──────────────────┘         └─────────────┘       │
│          │                      │                           │              │
│          │                      ▼                           │              │
│          │              ┌──────────────────┐                │              │
│          │              │   Your Notes     │                │              │
│          │              │  ┌────────────┐  │                │              │
│          │              │  │ Research   │  │                │              │
│          └─────────────►│  │ Analysis   │◄─┼────────────────┘              │
│                         │  │ Diagrams   │  │                               │
│                         │  └────────────┘  │                               │
│                         └──────────────────┘                               │
│                                                                             │
│     Agent writes → You read → Agent updates → You collaborate              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Key design principles:**

1. **API-First** — Full CRUD access via REST. No UI required for agents.
2. **Pay-Per-Call** — $0.001/read, $0.005/write. No subscriptions.
3. **Visual Outputs** — Tables, diagrams, flowcharts rendered beautifully.
4. **Lightweight** — Not a heavy document editor. A fast scratchpad.

## Feature 1: Full CRUD for AI Agents

LLMs using [seren-MCP](https://serendb.com/mcp) get unrestricted access to create, read, update, and delete notes. No permission dialogs. No "Are you sure?" prompts. Your agent manages your workspace like any other API resource.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     FULL CRUD ACCESS FOR LLMS                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌─────────────────────┐                                                   │
│   │ POST /workspaces/   │                                                   │
│   │      my-workspace/  │ ─────────► CREATE note "market-research"          │
│   │      documents      │                                                   │
│   └─────────────────────┘                                                   │
│                                      ▼                                      │
│   ┌─────────────────────┐  ┌──────────────────────────────────┐            │
│   │ GET /documents/     │  │  📄 market-research               │            │
│   │     {id}            │──│  Created: 2026-01-14 09:15:00    │            │
│   └─────────────────────┘  │  Author: claude-agent-7x2k       │            │
│                            │                                  │            │
│   ┌─────────────────────┐  │  # Q1 Market Analysis            │            │
│   │ PUT /documents/     │  │  - Competitor A: 23% share       │            │
│   │     {id}            │──│  - Competitor B: 18% share       │            │
│   └─────────────────────┘  │  - Our position: Growing...      │            │
│                            └──────────────────────────────────┘            │
│   ┌─────────────────────┐                                                   │
│   │ DELETE /documents/  │ ─────────► REMOVE when analysis complete          │
│   │        {id}         │                                                   │
│   └─────────────────────┘                                                   │
│                                                                             │
│   No permission prompts. No confirmation dialogs. Just API calls.           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Why this matters:** Your agent can maintain working memory across sessions. Start a research task, pause, resume later—the notes persist. The agent can organize its own workspace, archive completed analyses, and clean up temporary files without asking you.

### Example: Agent-Managed Research Pipeline

```json
// Agent creates workspace for ongoing project
mcp__seren_notes__create_document {
  "workspace": "competitive-intel",
  "title": "Weekly Competitor Scan",
  "content": "## Week of 2026-01-14\n\nTracking: RapidAPI, Hugging Face, Replicate\n\n### Pending Analysis\n- [ ] HN mentions\n- [ ] Pricing changes\n- [ ] New features"
}

// Agent updates as it gathers data
mcp__seren_notes__update_document {
  "document_id": "doc_abc123",
  "content": "## Week of 2026-01-14\n\n### HN Mentions\n- RapidAPI: 3 posts, 142 total upvotes\n- Hugging Face: 7 posts, 891 total upvotes ⚠️ HIGH\n..."
}

// Agent archives when complete
mcp__seren_notes__move_document {
  "document_id": "doc_abc123",
  "destination": "archive/2026-01"
}
```

## Feature 2: Pay-Per-Call Cost Control

[SerenAI](https://serendb.com) agents already understand micropayments. Every API call on our platform has a price, and agents budget accordingly. Seren Notes extends this model to note-taking.

| Operation | Cost | Use Case |
|-----------|------|----------|
| Read | $0.001 | Retrieve existing notes, check status |
| Write | $0.005 | Create or update documents |
| Search | $0.002 | Full-text search across workspace |
| Delete | Free | Cleanup costs nothing |

**Why pay-per-call?** Because you can instruct your LLM to manage costs.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     LLM COST AWARENESS                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   USER PROMPT:                                                              │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │ "Research the top 10 DeFi protocols. Keep costs under $0.10.        │  │
│   │  Save your findings to Seren Notes for me to review."               │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│   LLM REASONING:                                                            │
│   ┌─────────────────────────────────────────────────────────────────────┐  │
│   │ Budget: $0.10                                                       │  │
│   │ CoinGecko queries: 10 × $0.001 = $0.01                              │  │
│   │ Perplexity analysis: 3 × $0.005 = $0.015                            │  │
│   │ Seren Notes writes: 5 × $0.005 = $0.025                             │  │
│   │ ─────────────────────────────────                                   │  │
│   │ Total: $0.05 ✓ Under budget                                         │  │
│   │                                                                     │  │
│   │ I'll batch my findings into fewer documents to minimize writes.     │  │
│   └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
│   Your LLM optimizes for cost automatically. No surprise bills.             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

This is the SerenAI philosophy: **agents should understand the cost of their actions.** When every operation has a price, agents naturally optimize. They batch updates. They cache reads. They clean up after themselves.

## Feature 3: Visual Data Rendering

[Seren Data](https://serendb.com/data) publishers give your agents access to databases and APIs—but the outputs are raw JSON. Users want to *see* how their LLM interprets data, not scroll through nested objects.

Seren Notes renders structured content beautifully:

- **Tables** — Markdown tables become sortable, styled grids
- **Diagrams** — Mermaid.js syntax renders as flowcharts and sequence diagrams
- **Charts** — Simple charting for numerical comparisons
- **Code blocks** — Syntax-highlighted with copy buttons

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     FROM RAW DATA TO VISUAL INSIGHT                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   AGENT QUERIES COINGECKO API:                                              │
│   ┌───────────────────────────────────────────────────────────────┐        │
│   │ { "bitcoin": { "usd": 97500, "usd_24h_change": 2.3 },        │        │
│   │   "ethereum": { "usd": 3850, "usd_24h_change": -1.2 },       │        │
│   │   "solana": { "usd": 142, "usd_24h_change": 5.7 } ... }      │        │
│   └───────────────────────────────────────────────────────────────┘        │
│                                    │                                        │
│                                    ▼                                        │
│   AGENT WRITES TO SEREN NOTES:                                              │
│   ┌───────────────────────────────────────────────────────────────┐        │
│   │  # Daily Crypto Snapshot                                      │        │
│   │                                                               │        │
│   │  | Asset    | Price    | 24h Change |                        │        │
│   │  |----------|----------|------------|                        │        │
│   │  | Bitcoin  | $97,500  | +2.3% ▲    |                        │        │
│   │  | Ethereum | $3,850   | -1.2% ▼    |                        │        │
│   │  | Solana   | $142     | +5.7% ▲    |                        │        │
│   │                                                               │        │
│   │  ```mermaid                                                   │        │
│   │  pie title Market Dominance                                   │        │
│   │      "BTC" : 54                                               │        │
│   │      "ETH" : 18                                               │        │
│   │      "SOL" : 4                                                │        │
│   │      "Other" : 24                                             │        │
│   │  ```                                                          │        │
│   └───────────────────────────────────────────────────────────────┘        │
│                                    │                                        │
│                                    ▼                                        │
│   USER SEES IN SEREN NOTES:                                                 │
│   ┌───────────────────────────────────────────────────────────────┐        │
│   │  ╔════════════════════════════════════════════╗               │        │
│   │  ║     📊 Daily Crypto Snapshot               ║               │        │
│   │  ╠════════════════════════════════════════════╣               │        │
│   │  ║  ┌─────────┬──────────┬────────────┐      ║               │        │
│   │  ║  │ Asset   │ Price    │ 24h Change │      ║               │        │
│   │  ║  ├─────────┼──────────┼────────────┤      ║               │        │
│   │  ║  │ Bitcoin │ $97,500  │ ▲ +2.3%    │      ║               │        │
│   │  ║  │ Ethereum│ $3,850   │ ▼ -1.2%    │      ║               │        │
│   │  ║  │ Solana  │ $142     │ ▲ +5.7%    │      ║               │        │
│   │  ║  └─────────┴──────────┴────────────┘      ║               │        │
│   │  ║          [Rendered Pie Chart]              ║               │        │
│   │  ╚════════════════════════════════════════════╝               │        │
│   └───────────────────────────────────────────────────────────────┘        │
│                                                                             │
│   Not a heavy document. A lightweight, visual workspace.                    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

This is the "temporary space" users have been asking for. Your agent can create a quick comparison table, render a decision flowchart, or build a data dashboard—all viewable at [notes.serendb.com](https://notes.serendb.com) without installing anything.

## Getting Started

### Install seren-MCP

Add the SerenAI MCP server to Claude Code with one command:

```bash
claude mcp add --scope user --transport http seren https://mcp.serendb.com/mcp
```

This gives Claude access to all [SerenAI publishers](https://serendb.com/bestsellers), including Seren Notes.

### Create Your First Note

```json
mcp__seren_notes__create_document {
  "workspace": "default",
  "title": "Agent Scratchpad",
  "content": "# Working Memory\n\nThis is where I'll store intermediate results."
}
```

### View Your Notes

Open [notes.serendb.com](https://notes.serendb.com) in any browser. Sign in with your Seren account. Your agent's notes appear in real-time.

## Pricing

| Storage | API Calls | Cost |
|---------|-----------|------|
| First 5 GB | — | Free |
| Additional storage | — | $0.35/GB per month |
| Document read | — | $0.001 per call |
| Document write | — | $0.005 per call |
| Search query | — | $0.002 per query |

Pay only for what you use via [SerenBucks](https://serendb.com/docs/serenbucks) (prepaid credits). No subscriptions, no monthly minimums.

## Features Available Today

Seren Notes ships with everything you need to get started:

- **Migration tools** — Import from Notion and Evernote ($25 one-time fee)
- **Collaborative editing** — Multiple agents can work in one workspace
- **Export options** — Markdown, JSON, PDF for all your documents
- **Real-time sync** — Changes appear instantly in the web UI
- **Full-text search** — Find anything across all your workspaces

Built on [AppFlowy](https://appflowy.io) (AGPLv3), your data is yours. Self-host if you want full control, or use our managed service at [notes.serendb.com](https://notes.serendb.com).

---

> **Get early access** — Sign up at **[console.serendb.com/signup](https://console.serendb.com/signup)** with invite code **`serennotes2026`**

*Questions? Email hello@serendb.com or join our [Discord](https://discord.gg/jseg7q4KS7).*

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
║              Payment Infrastructure for the AI Agent Economy                 ║
║                                                                              ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  TypeScript  •  Rust  •  Cloudflare Workers  •  Base  •  x402   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     SEREN NOTES STACK:                                                       ║
║     📝 API-First  •  💰 Pay-Per-Call  •  📊 Visual Rendering  •  🤖 LLM-Native ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: January 14, 2026*
*Tags: #SerenNotes #SerenAI #x402 #AIAgents #Scratchpads #NoteTaking #DeveloperTools #MCP*
