# Show HN: Seren Desktop – Open Source AI IDE with Publisher Marketplace and x402 Micropayments

Hey HN! We built Seren Desktop, an MIT-licensed desktop IDE with a built-in marketplace of 50+ AI-accessible services. Pay for what you use with USDC micropayments (x402 protocol on Base network).

## What makes it different

Most AI IDEs lock you into one provider, one model, and a subscription. Seren Desktop:

**Publisher Marketplace** – 50+ services AI can access directly:
- **Databases**: Query SerenDB (serverless Postgres), MongoDB, Supabase with natural language
- **Web tools**: Scrape websites (Firecrawl), AI search (Perplexity)
- **Integrations**: Email, calendar, CRM. Coming: GitHub, Linear, Notion
- **Compute**: Code sandboxes, agent templates, custom workflows

**Micropayments that work** – x402 USDC on Base network:
- No subscriptions, no tiers, no credits expiring
- Pay per API call at publisher-set prices
- Example: Firecrawl scrape = $0.01, Perplexity search = $0.005

**Open everything else**:
- MIT license, ~10MB Rust/Tauri binary (not Electron)
- Any AI model (Claude, GPT-4, Gemini) via Seren Gateway or your own keys
- Semantic codebase indexing with sqlite-vec (local, instant)
- Agent Client Protocol support (run Claude Code directly)

## Why we built this

We kept hitting the same problem: AI assistants need external services but have no way to pay for them. Want to scrape a website? Search the web? Query a database? You're stuck copy-pasting or building custom integrations.

Cursor and Copilot are great for editing code, but they can't access external services. Continue.dev has MCP but no payment system. We wanted:

1. **A real marketplace** where publishers can offer services and get paid automatically
2. **Micropayments** that actually work (no minimum balances, instant settlement)
3. **Open source client** so you're not locked in
4. **Semantic codebase indexing** so AI understands your project

## How semantic indexing works

When you open a project:
1. Seren walks your files (respecting .gitignore)
2. Chunks code at function/class boundaries (Rust, TypeScript/JavaScript, Python)
3. Generates embeddings via SerenEmbed API (paid via SerenBucks micropayments)
4. Stores vectors locally in sqlite-vec for instant KNN search
5. File watcher triggers incremental re-indexing on save

When you chat with AI:
- Before sending your message, Seren queries the local vector store for relevant code chunks
- Automatically injects the most similar functions/classes into the AI's context
- No manual copy-pasting needed – the AI just *knows* your codebase

## Architecture

The client is **fully open source** (MIT). It connects to Seren's Gateway API for:
- Authentication & billing (SerenBucks prepaid credit system)
- AI model routing (Claude, GPT, Gemini)
- Publisher marketplace (Firecrawl, Perplexity, databases, email)
- SerenEmbed API (embedding generation)

Think of it like VS Code (open source) connecting to the Extension Marketplace (proprietary).

## Tech stack

- **Frontend**: SolidJS + TypeScript + Vite (not React!)
- **Backend**: Rust + Tauri 2.0
- **Editor**: Monaco 0.52+
- **Vector store**: sqlite-vec (not pgvector or Pinecone – all local)
- **Agent protocol**: agent-client-protocol + claude-code-acp-rs
- **Crypto**: alloy-rs for USDC micropayments on Base network (x402 standard)

## What makes this different from other AI IDEs

**vs Cursor/Windsurf:**
- Publisher marketplace with real micropayments (they don't have this)
- Fully open source (MIT), not proprietary
- Works with any AI model, not locked to one provider

**vs GitHub Copilot:**
- AI can access external services (web scraping, databases, APIs)
- Publisher marketplace with 50+ services
- Pay-per-use, not subscription
- Semantic codebase indexing

**vs Continue.dev:**
- Built-in marketplace + micropayments (they have MCP but no payment layer)
- Native desktop app (Tauri), not VS Code extension
- Agent Client Protocol support for Claude Code

## Current status

- Alpha release (v0.1.0-alpha.9)
- Builds for macOS (Apple Silicon + Intel), Windows, Linux
- Free tier included (Gemini 2.0 Flash)
- Active development on GitHub

## Try it

- GitHub: https://github.com/serenorg/seren-desktop
- Website: https://serendb.com
- Docs: https://docs.serendb.com

We'd love feedback from the HN community! We're especially interested in:
- What other languages should we support for smart chunking?
- Should we add local LLM support (Ollama, LM Studio)?
- What MCP servers or publishers would you want to see?

---

**Note:** The embedding API (SerenEmbed) is paid via SerenBucks micropayments because generating embeddings costs money. But the entire client is MIT licensed – you can fork it, self-host it, or connect to your own embedding service if you prefer.

Taariq Lewis, SerenAI, Paloma, and Volume at https://serendb.com
