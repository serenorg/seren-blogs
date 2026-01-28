# Show HN: Seren Desktop – Open Source AI IDE with Semantic Code Search

Hey HN! We built Seren Desktop, a lightweight AI-powered desktop IDE that indexes your codebase with semantic embeddings so AI actually understands your project context.

## The problem

AI coding assistants can't see your entire codebase, so they give generic answers or miss important context. We wanted something that:
- Indexes the entire codebase semantically (not just grep)
- Works with any AI model (Claude, GPT-4, Gemini, or your own keys)
- Runs AI agents directly (Claude Code via Agent Client Protocol)
- Stays lightweight (~10MB, not Electron)

## How it works

1. Open a project → Seren indexes your code with SerenEmbed embeddings
2. Stores vectors locally in sqlite-vec for instant similarity search
3. When you chat with AI, automatically injects relevant code chunks into context
4. File watcher triggers incremental re-indexing on save

No manual copy-pasting. The AI just knows your codebase.

## What's inside

- **Semantic indexing** with language-aware chunking (Rust, TS/JS, Python)
- **Monaco editor** with Cmd+K inline AI editing
- **Agent Client Protocol** for Claude Code and compatible agents
- **Publisher marketplace** – 50+ AI-accessible services (databases, web scraping, email)
- **Model Context Protocol** – Connect to MCP servers for extended capabilities
- **Micropayments** – x402 USDC on Base network for AI services

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
