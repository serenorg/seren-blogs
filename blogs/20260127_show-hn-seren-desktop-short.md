# Show HN: Seren Desktop – Open Source AI IDE with Publisher Marketplace and x402 Micropayments

Hey HN! We built Seren Desktop, an MIT-licensed desktop IDE with a built-in marketplace of 50+ AI-accessible services. Pay for what you use with USDC micropayments on Base (x402 protocol).

## What makes it different

Most AI IDEs lock you into one provider and one model. Seren Desktop:
- **Open marketplace** – 50+ publishers for databases (SerenDB, MongoDB), web scraping (Firecrawl), AI search (Perplexity), email, calendars, CRM
- **Micropayments that work** – x402 USDC on Base network. No subscription tiers, no credits expiring. Pay per API call.
- **Any AI model** – Claude, GPT-4, Gemini via Seren Gateway, or bring your own API keys
- **Fully open source** – MIT license, ~10MB Rust/Tauri binary (not Electron)
- **Semantic codebase indexing** – sqlite-vec + SerenEmbed for instant code context retrieval

## The marketplace

Browse 50+ publishers in the app:

- **Databases**: Query SerenDB (serverless Postgres), MongoDB, Supabase with natural language. AI generates SQL/queries automatically.
- **Web tools**: Scrape websites with Firecrawl, AI-powered search with Perplexity
- **Integrations**: Email, calendar, CRM actions. Coming soon: GitHub, Linear, Notion
- **Compute**: Run code sandboxes, agent templates, custom workflows

Each publisher sets their own pricing. You pay per API call with USDC on Base (x402 standard). No subscriptions, no expiring credits.

## How semantic indexing works

1. Index your codebase with SerenEmbed (language-aware chunking for Rust, TS/JS, Python)
2. Vectors stored locally in sqlite-vec for instant retrieval
3. AI automatically gets relevant code context during chat
4. File watcher triggers incremental re-indexing on save

## What's inside

- **Monaco editor** with Cmd+K inline AI editing
- **Agent Client Protocol** for running Claude Code and compatible agents
- **Model Context Protocol** for local MCP servers
- **Multi-model support** with free tier (Gemini 2.0 Flash)

## Tech stack

- Frontend: SolidJS + TypeScript + Vite
- Backend: Rust + Tauri 2.0
- Vector store: sqlite-vec (all local, no cloud)
- MIT licensed, ~10MB binary

## Architecture

The client is fully open source. It connects to Seren Gateway for:
- AI model routing & authentication
- SerenEmbed API (embedding generation)
- Publisher marketplace

Think VS Code (open source) + Extension Marketplace (proprietary).

## Try it

- GitHub: https://github.com/serenorg/seren-desktop
- Website: https://serendb.com
- Current: v0.1.0-alpha.9 (macOS, Windows, Linux)

We'd love your feedback on:
- What languages should we support for smart chunking?
- Should we add local LLM support (Ollama, LM Studio)?
- What MCP servers or publishers would you want?

Taariq Lewis, SerenAI, Paloma, and Volume at https://serendb.com
