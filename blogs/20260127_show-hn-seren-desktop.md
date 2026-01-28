# Show HN: Seren Desktop – Open Source AI IDE with Semantic Code Search

Hey HN! We built Seren Desktop, a lightweight (~10MB) AI-powered desktop IDE that indexes your entire codebase with semantic embeddings so AI actually understands your project context.

## What is it?

Seren Desktop is an MIT-licensed Tauri + SolidJS desktop app that combines:
- **AI chat with automatic code context** – The AI retrieves relevant code chunks from your indexed codebase during conversations
- **Monaco editor** – Full VS Code editing experience with Cmd+K inline AI editing
- **Agent Client Protocol (ACP)** – Run Claude Code and compatible AI agents directly in the app
- **Semantic codebase indexing** – Local sqlite-vec storage for instant similarity search
- **Publisher marketplace** – Access 50+ AI-accessible services (databases, web scraping, email, etc.)
- **Model Context Protocol (MCP)** – Connect to MCP servers for extended AI capabilities

## Why we built this

We kept hitting the same frustration: AI assistants couldn't see our entire codebase, so they'd give generic answers or miss important context. Tools like Cursor and GitHub Copilot are great, but we wanted something open source that could:

1. **Index the entire codebase semantically** – Not just grep/fuzzy search, but actual vector embeddings that understand code relationships
2. **Work with any AI model** – Claude, GPT-4, Gemini, or your own API keys
3. **Run AI agents directly** – Support the Agent Client Protocol so tools like Claude Code can read files, execute commands, and make edits
4. **Stay lightweight and fast** – ~10MB binary, not a bloated Electron app

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

## What makes this different

**vs Cursor/Windsurf:**
- Fully open source (MIT), not proprietary
- Works with any AI model, not locked to one provider
- ACP agent support built-in
- Publisher marketplace for extended capabilities

**vs GitHub Copilot:**
- Full IDE, not just an editor extension
- Semantic codebase indexing with vector search
- Multi-model support (not just OpenAI)
- Local-first architecture

**vs Continue.dev:**
- Native desktop app (Tauri), not VS Code extension
- Built-in publisher marketplace and MCP integration
- Micropayments for AI-powered services
- Agent Client Protocol support

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
