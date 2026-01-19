# CoinGecko is Now Live on SerenAI: Pay-Per-Call Crypto Data for AI Agents

```text
          .-"""-.
         /        \
        |  O    O |      ____      _         ____   / _ \___  | |  ___   ___ 
        |    __   |    / ___|___ (_)_ __   / ___|  | | / _ \ | | / _ \ / _ \
         \  \__/  /   | |   / _ \| | '_ \ | |  _   | ||  __/ | || (_) |  __/
          '-.  .-'     | |__| (_) | | | | || |_| |  | | \___|_|_| \___/ \___|
            ||        \____\___/|_|_| |_|\____|  |_|          |_____|
           /||\
          / || \
         /  ||  \
        '   "'   '

         S E R E N A I   x 4 0 2   G A T E W A Y
              Pay-Per-Call Crypto Data

            $0.0005 per API call
```

> **Special Thanks:** We extend our sincere gratitude to the CoinGecko team for being a founding financial-data partner and publisher on SerenAI. Their early commitment to the pay-per-call model for AI agents is helping pioneer a new era of accessible, democratized crypto market data.

**Your AI agent no longer needs a $129/month subscription to access premium crypto market data.**

We're excited to announce that CoinGecko—one of the world's largest cryptocurrency data aggregators—is now available as a data publisher on the SerenAI x402 Gateway. AI agents can now access 50+ market data endpoints and 2 years of historical data, paying only for what they use via USDC micropayments on Base.

**👉 [View CoinGecko on SerenDB](https://serendb.com/bestsellers/a42f2c7f-c075-488f-a3f9-acfd7304934b)**

```text
    ╔═══════════════════════════════════════════════════════════════╗
    ║  AI AGENT  ──────►  SerenAI x402  ──────►  CoinGecko API     ║
    ║     🤖              Gateway 💳              📊               ║
    ║                                                               ║
    ║     $0.0005 USDC ═══════════════════════════►  Real-time     ║
    ║     per call                                   Crypto Data   ║
    ╚═══════════════════════════════════════════════════════════════╝
```

## The Numbers Tell the Story

Since launching on December 2nd, CoinGecko on SerenAI has already processed:

- **2,998 transactions** from AI agents
- **2 unique agents served** in the first week
- **$0.0005 per API call** (0.05 cents)

At half a cent per call, an agent would need to make **258,000 API calls per month** before a traditional $129/month CoinGecko Pro subscription becomes more economical. For most AI trading agents running momentum strategies, that's roughly 6 calls per minute, 24/7—far more than typical use cases require.

```text
         ┌─────────────────────────────────────────────────────────┐
         │   📈  FIRST WEEK STATS                                  │
         ├─────────────────────────────────────────────────────────┤
         │                                                         │
         │    TRANSACTIONS    AGENTS      PRICE/CALL               │
         │    ═══════════     ══════      ══════════               │
         │                                                         │
         │       2,998          2         $0.0005                  │
         │        ███           █          ▓                       │
         │        ███           █          ▓                       │
         │        ███           █          ▓                       │
         │        ███                      ▓                       │
         │        ███                                              │
         │                                                         │
         └─────────────────────────────────────────────────────────┘
```

## Why Micropayments Win for AI Agents

Traditional API subscriptions assume human developers building applications with predictable traffic. AI agents operate differently:

- **Burst patterns**: An agent might make 500 calls during a market event, then go quiet for hours
- **Experimental workloads**: Testing a new strategy shouldn't require committing to monthly fees
- **Multi-source strategies**: Agents often need data from multiple providers—subscriptions don't scale

With SerenAI's x402 protocol, your agent pays exactly $0.0005 per CoinGecko call. Run 1,000 calls this month? That's 50 cents. Run 10 calls? Half a penny. No minimums, no commitments, no wasted spend.

```text
    ┌────────────────────────────────────────────────────────────────┐
    │                                                                │
    │    SUBSCRIPTION MODEL          vs       PAY-PER-CALL MODEL    │
    │    ══════════════════                   ══════════════════     │
    │                                                                │
    │    ┌──────────────────┐              ┌──────────────────┐     │
    │    │  $129/month      │              │  $0.0005/call    │     │
    │    │  ████████████████│              │  ▓               │     │
    │    │  ████████████████│              │                  │     │
    │    │  ████████████████│              │  Pay what you    │     │
    │    │  Fixed cost even │              │  use. Nothing    │     │
    │    │  if unused       │              │  more.           │     │
    │    └──────────────────┘              └──────────────────┘     │
    │                                                                │
    │         😰 WASTE                          😎 EFFICIENT         │
    │                                                                │
    └────────────────────────────────────────────────────────────────┘
```

## Building a Market-Wide Momentum Scanner

Let's walk through a practical example: building an AI agent that scans the **entire crypto market** for momentum opportunities using CoinGecko's **paid-only** endpoints, the Seren MCP server, and SerenDB's free tier for state persistence.

**Why paid endpoints?** CoinGecko's free API only gives you data for coins you already know about. The paid **Top Gainers/Losers** endpoint scans all 10,000+ coins and returns the biggest movers—data you simply cannot get for free.

```text
    ╔══════════════════════════════════════════════════════════════════════╗
    ║                                                                      ║
    ║     🤖  M A R K E T - W I D E   M O M E N T U M   S C A N N E R     ║
    ║                                                                      ║
    ║     ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ║
    ║     │  Claude  │───►│  SerenAI │───►│CoinGecko │───►│  SerenDB │    ║
    ║     │  Desktop │    │   MCP    │    │ PRO API  │    │   State  │    ║
    ║     └──────────┘    └──────────┘    └──────────┘    └──────────┘    ║
    ║          │                                               │          ║
    ║          │     SCAN 10K+ COINS ─► RANK ─► STORE         │          ║
    ║          └───────────────────────────────────────────────┘          ║
    ║                                                                      ║
    ╚══════════════════════════════════════════════════════════════════════╝
```

### Step 1: Install and Configure the Seren MCP Server

The Seren MCP server runs **locally on your machine**—we never store your private keys. Choose your AI coding tool below:

---

#### Claude Desktop

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "serenai": {
      "command": "npx",
      "args": ["-y", "@serenai/mcp-server"],
      "env": {
        "SERENAI_AGENT_WALLET": "0xYourAgentWallet",
        "SERENAI_PRIVATE_KEY": "your-agent-private-key"
      }
    }
  }
}
```

Restart Claude Desktop to activate.

---

#### Claude Code CLI (Anthropic)

Run this command in your terminal:

```bash
claude mcp add --transport stdio serenai \
  --env SERENAI_AGENT_WALLET=0xYourAgentWallet \
  --env SERENAI_PRIVATE_KEY=your-agent-private-key \
  -- npx -y @serenai/mcp-server
```

Or add to `~/.claude.json` (user scope) or `.mcp.json` (project scope):

```json
{
  "mcpServers": {
    "serenai": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@serenai/mcp-server"],
      "env": {
        "SERENAI_AGENT_WALLET": "0xYourAgentWallet",
        "SERENAI_PRIVATE_KEY": "your-agent-private-key"
      }
    }
  }
}
```

Verify with `claude mcp list`.

---

#### Codex CLI (OpenAI)

Run this command in your terminal:

```bash
codex mcp add serenai -- npx -y @serenai/mcp-server
```

Or add to `~/.codex/config.toml`:

```toml
[mcp_servers.serenai]
command = "npx"
args = ["-y", "@serenai/mcp-server"]

[mcp_servers.serenai.env]
SERENAI_AGENT_WALLET = "0xYourAgentWallet"
SERENAI_PRIVATE_KEY = "your-agent-private-key"
```

Verify with `codex mcp list`.

---

#### Gemini CLI (Google)

Run this command in your terminal:

```bash
gemini mcp add serenai -- npx -y @serenai/mcp-server
```

Or add to `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "serenai": {
      "command": "npx",
      "args": ["-y", "@serenai/mcp-server"],
      "env": {
        "SERENAI_AGENT_WALLET": "0xYourAgentWallet",
        "SERENAI_PRIVATE_KEY": "$SERENAI_PRIVATE_KEY"
      }
    }
  }
}
```

Verify with `gemini mcp list`.

---

```text
    ╔══════════════════════════════════════════════════════════════════════╗
    ║                                                                      ║
    ║     🔧 MCP SERVER CONFIGURATION BY TOOL                              ║
    ║                                                                      ║
    ║     ┌──────────────┐  ┌──────────────┐  ┌──────────────┐            ║
    ║     │Claude Desktop│  │ Claude Code  │  │   Codex CLI  │            ║
    ║     │   (JSON)     │  │ (JSON/.mcp)  │  │   (TOML)     │            ║
    ║     └──────────────┘  └──────────────┘  └──────────────┘            ║
    ║                                                                      ║
    ║     ┌──────────────┐                                                ║
    ║     │  Gemini CLI  │   All tools use the same MCP protocol          ║
    ║     │   (JSON)     │   Your private keys stay LOCAL                 ║
    ║     └──────────────┘                                                ║
    ║                                                                      ║
    ╚══════════════════════════════════════════════════════════════════════╝
```

### Step 2: Scan the Entire Market with Top Gainers/Losers (Paid Only)

The **`/coins/top_gainers_losers`** endpoint is exclusive to CoinGecko's paid API plans. It scans all 10,000+ cryptocurrencies and returns the top 30 biggest movers by price change—impossible to replicate with the free API.

Copy this prompt into your AI coding tool (Claude Desktop, Claude Code, Codex, or Gemini):

> **Prompt:** "Use the SerenAI MCP server to fetch CoinGecko's top gainers and losers for the past 24 hours. Return the top 10 coins with the largest positive price movements."

Your agent will call the x402 Gateway, sign a $0.0005 USDC payment, and return:

```json
{
  "top_gainers": [
    { "id": "pepe", "symbol": "PEPE", "price_change_percentage_24h": 47.2 },
    { "id": "bonk", "symbol": "BONK", "price_change_percentage_24h": 32.8 },
    { "id": "floki", "symbol": "FLOKI", "price_change_percentage_24h": 28.1 },
    { "id": "wojak", "symbol": "WOJAK", "price_change_percentage_24h": 24.6 }
  ]
}
```

**This data is unavailable on the free API.** Without the paid endpoint, you'd have to manually query each of 10,000+ coins individually—an impossible task.

---

### Step 3: Store Strategy State in SerenDB Free Tier

> **🚀 SIGN UP FOR FREE:** Create your SerenDB account at **[console.serendb.com/signup](https://console.serendb.com/signup)**
> 
> **Use invite code: `serenai2025`** for immediate access to the free tier.

Once you have your SerenDB connection string, copy this prompt to set up your trading database:

> **Prompt:** "Connect to my SerenDB database and create a momentum_signals table with the following columns: id (auto-increment primary key), timestamp (default to now), asset (text), price (decimal 18,8), change_24h (decimal 8,4), volume (decimal 24,2), signal (text for BUY/SELL/HOLD), and confidence (decimal 5,4). Then confirm the table was created."

Claude will execute:

```sql
CREATE TABLE momentum_signals (
  id SERIAL PRIMARY KEY,
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  asset TEXT,
  price DECIMAL(18,8),
  change_24h DECIMAL(8,4),
  volume DECIMAL(24,2),
  signal TEXT,
  confidence DECIMAL(5,4)
);
```

### Step 4: Run the Market-Wide Momentum Scanner (Paid Only)

Copy this complete strategy prompt into your AI coding tool:

> **Prompt:** "Run a market-wide momentum scan using CoinGecko's paid API via SerenAI:
>
> 1. Fetch the top 30 gainers and losers from CoinGecko's /coins/top_gainers_losers endpoint (paid-only)
> 2. Filter to coins with market cap > $10 million (avoid low-liquidity traps)
> 3. Rank the top 5 gainers by a momentum score = price_change_24h × sqrt(market_cap / 1B)
> 4. Store each coin's signal in my SerenDB momentum_signals table with: asset, price, change_24h, market_cap, momentum_score, signal='WATCHLIST'
> 5. Show me the top 5 momentum plays with reasoning"

Your agent will scan all 10,000+ coins, filter, rank, store, and explain—all for $0.0005.

```text
    ┌──────────────────────────────────────────────────────────────────────┐
    │                                                                      │
    │     📊 MARKET-WIDE MOMENTUM SCANNER (PAID-ONLY DATA)                 │
    │     ════════════════════════════════════════════════                 │
    │                                                                      │
    │     STEP 1: Scan ALL 10,000+ coins via /top_gainers_losers          │
    │         ┌─────────────────────────────────────┐                      │
    │         │  🔍 CoinGecko Pro API scans entire  │                      │
    │         │  market - impossible with free API  │                      │
    │         └─────────────────────────────────────┘                      │
    │                                                                      │
    │     STEP 2: Filter by market cap > $10M                             │
    │         ┌─────────────────────────────────────┐                      │
    │         │  🛡️ Avoid low-liquidity pump & dumps │                      │
    │         └─────────────────────────────────────┘                      │
    │                                                                      │
    │     STEP 3: Rank by momentum_score                                   │
    │         ┌─────────────────────────────────────┐                      │
    │         │  📈 score = change × sqrt(mcap/1B)  │                      │
    │         │  Balances momentum with liquidity   │                      │
    │         └─────────────────────────────────────┘                      │
    │                                                                      │
    │     STEP 4: Store top 5 in SerenDB                                   │
    │         ┌─────────────────────────────────────┐                      │
    │         │  💾 Persistent watchlist for agent  │                      │
    │         └─────────────────────────────────────┘                      │
    │                                                                      │
    │     💡 WHY PAID? Free API requires querying each coin individually.  │
    │        With 10,000+ coins, that's 10,000+ calls = $5.00              │
    │        Paid endpoint: 1 call = $0.0005 (10,000x cheaper!)           │
    │                                                                      │
    └──────────────────────────────────────────────────────────────────────┘
```

## 🎁 Bonus: Santa Gecko's Volatility-Adjusted Scanner

```text
    ╔══════════════════════════════════════════════════════════════════════╗
    ║                                                                      ║
    ║           *    .  *       .            *        .    *               ║
    ║       *       .       *        .   ★          .       .    *         ║
    ║                   .         *             .                          ║
    ║                      🎄                                              ║
    ║                     /|\                                              ║
    ║                    /|||\        .-"""-.                              ║
    ║                   /||||||\     /   🎅  \     H O - H O - H O !       ║
    ║                  /|||🎁|||\   |  O    O |                            ║
    ║                 /||||||||||\ |    __   |    Merry Crypto-mas!        ║
    ║                     |||       \  \__/  /    Here's a gift for        ║
    ║                     |||        '-.  .-'     your AI agents...        ║
    ║                  ___|||___        ||                                 ║
    ║                 |_________|      /||\      🦎 SANTA GECKO 🦎         ║
    ║                                 / || \                               ║
    ║                                '  ''  '                              ║
    ║                                                                      ║
    ║         V O L A T I L I T Y - A D J U S T E D   S C A N N E R       ║
    ║                                                                      ║
    ╚══════════════════════════════════════════════════════════════════════╝
```

**The gift that keeps on giving:** A sophisticated scanner that finds coins that beat their own volatility drag—the true outperformers.

### Why Volatility-Adjusted Returns?

Raw returns lie. A coin up 200% with 500% volatility is actually **destroying value** after accounting for volatility drag. Santa Gecko's scanner finds the coins that truly outperform:

```text
    ┌────────────────────────────────────────────────────────────────────┐
    │                                                                    │
    │   📉 VOLATILITY DRAG EXPLAINED                                     │
    │   ═══════════════════════════                                      │
    │                                                                    │
    │   Volatility drag = (volatility²) / 2                              │
    │                                                                    │
    │   EXAMPLE:                                                         │
    │   ┌─────────────────────────────────────────────────────────┐     │
    │   │  Coin A: +150% return, 80% volatility                   │     │
    │   │  Vol drag = (0.80²) / 2 = 32%                           │     │
    │   │  Risk-adjusted = 150% - 32% = +118% ✅ OUTPERFORMER     │     │
    │   ├─────────────────────────────────────────────────────────┤     │
    │   │  Coin B: +200% return, 300% volatility                  │     │
    │   │  Vol drag = (3.00²) / 2 = 450%                          │     │
    │   │  Risk-adjusted = 200% - 450% = -250% ❌ VALUE DESTROYER │     │
    │   └─────────────────────────────────────────────────────────┘     │
    │                                                                    │
    │   🎯 Santa's secret: Find the coins where return > vol drag        │
    │                                                                    │
    └────────────────────────────────────────────────────────────────────┘
```

### The Santa Gecko Scanner Prompt

Copy this complete strategy into your AI coding tool:

> **Prompt:** "Run a daily volatility-adjusted return scan using CoinGecko via SerenAI:
>
> 1. First, connect to my SerenDB and ensure my momentum_signals table has these columns (add if missing): ttm_return (decimal 10,4), est_volatility (decimal 10,4), vol_drag (decimal 10,4), risk_adj_score (decimal 10,4)
> 2. Fetch top 250 coins from /coins/markets with these params: vs_currency=usd, order=market_cap_desc, price_change_percentage=1y, per_page=250
> 3. Filter criteria: Market cap > $100M (liquidity floor), 1-year price change > 0% (TTM winners only)
> 4. For each winner, calculate: Estimated annual volatility = |30d_change| × √12, Volatility drag = (volatility²) / 2, Risk-adjusted score = TTM_return - volatility_drag
> 5. Store in SerenDB momentum_signals table: asset, price, ttm_return, est_volatility, vol_drag, risk_adj_score, signal = 'OUTPERFORMER' if risk_adj_score > 0
> 6. Return top 10 ranked by risk_adjusted_score with: Asset category, Why it beat volatility drag"

### Extended SerenDB Schema

Your agent will automatically add these columns if needed:

```sql
ALTER TABLE momentum_signals ADD COLUMN IF NOT EXISTS ttm_return DECIMAL(10,4);
ALTER TABLE momentum_signals ADD COLUMN IF NOT EXISTS est_volatility DECIMAL(10,4);
ALTER TABLE momentum_signals ADD COLUMN IF NOT EXISTS vol_drag DECIMAL(10,4);
ALTER TABLE momentum_signals ADD COLUMN IF NOT EXISTS risk_adj_score DECIMAL(10,4);
```

```text
    ╔══════════════════════════════════════════════════════════════════════╗
    ║                                                                      ║
    ║   🎁 SANTA GECKO'S OUTPERFORMER SCANNER                              ║
    ║                                                                      ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 1: Ensure SerenDB schema has volatility columns    │      ║
    ║   │          ttm_return, est_volatility, vol_drag, score     │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                              ↓                                       ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 2: Fetch top 250 by market cap                     │      ║
    ║   │          /coins/markets with 1-year price change         │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                              ↓                                       ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 3: Filter for TTM winners                          │      ║
    ║   │          Market cap > $100M AND 1y_change > 0%           │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                              ↓                                       ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 4: Calculate volatility metrics                    │      ║
    ║   │          vol = |30d_change| × √12, drag = vol² / 2       │      ║
    ║   │          score = TTM_return - drag                       │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                              ↓                                       ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 5: Store OUTPERFORMERS in SerenDB                  │      ║
    ║   │          Only coins where score > 0                      │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                              ↓                                       ║
    ║   ┌──────────────────────────────────────────────────────────┐      ║
    ║   │  STEP 6: Return top 10 with analysis                     │      ║
    ║   │          "Why did this coin beat its volatility drag?"   │      ║
    ║   └──────────────────────────────────────────────────────────┘      ║
    ║                                                                      ║
    ║   🦎 From Santa Gecko with love - Happy Holidays! 🎄                 ║
    ║                                                                      ║
    ╚══════════════════════════════════════════════════════════════════════╝
```

---

## Cost Comparison: The SerenAI Advantage

This isn't just about saving money—it's about **building economically viable AI agents**.

| Usage Pattern | Calls/Month | SerenAI Cost | Subscription Cost | Savings | What This Means |
|---------------|-------------|--------------|-------------------|---------|-----------------|
| Hourly checks | 720 | **$0.36** | $129 | 99.7% | Monitor markets for less than a coffee |
| Every 15 min | 2,880 | **$1.44** | $129 | 98.9% | Active trading for pocket change |
| Every 5 min | 8,640 | **$4.32** | $129 | 96.7% | High-frequency signals under $5/month |
| Every minute | 43,200 | **$21.60** | $129 | 83.3% | Aggressive polling, still 83% cheaper |

**The real value?** Your agent can access CoinGecko, Nasdaq Data Link, U.S. Treasury, Polymarket, and Google Trends data—all through one gateway, all pay-per-call. No juggling five different subscriptions at $100+ each.

**For multi-source strategies**, the math becomes overwhelming in your favor:

- 5 data providers × $100/month = **$500/month** in subscriptions
- Same data via SerenAI at 1,000 calls each = **$2.50/month**

```text
    ╔════════════════════════════════════════════════════════════════════╗
    ║                                                                    ║
    ║  💰 COST COMPARISON: 5 DATA PROVIDERS                              ║
    ║                                                                    ║
    ║     TRADITIONAL SUBSCRIPTIONS        SERENAI PAY-PER-CALL         ║
    ║     ════════════════════════         ════════════════════          ║
    ║                                                                    ║
    ║     CoinGecko    $129 ████████       CoinGecko    $0.50 ▓         ║
    ║     Nasdaq       $100 ██████         Nasdaq       $0.50 ▓         ║
    ║     Treasury     $100 ██████         Treasury     $0.50 ▓         ║
    ║     Polymarket   $100 ██████         Polymarket   $0.50 ▓         ║
    ║     Google       $100 ██████         Google       $0.50 ▓         ║
    ║     ─────────────────────────        ─────────────────────         ║
    ║     TOTAL:      $529/month           TOTAL:      $2.50/month       ║
    ║                                                                    ║
    ║     📉 99.5% SAVINGS with SerenAI                                  ║
    ║                                                                    ║
    ╚════════════════════════════════════════════════════════════════════╝
```

## Available CoinGecko Endpoints

Through SerenAI, your agent can access CoinGecko's **Pro API endpoints**—features that require a $129+/month subscription elsewhere:

### Paid-Only Endpoints (The Real Value)

- **Top Gainers/Losers**: Market-wide momentum scan across 10,000+ coins
- **Recently Added Coins**: Latest 200 newly listed cryptocurrencies
- **OHLC Range**: Custom date range candlestick data for technical analysis
- **Global Market Cap Chart**: Historical total crypto market cap
- **NFT Market Data**: Floor prices, volume trends, and marketplace tickers

### Standard Endpoints

- **Price data**: Real-time and historical prices for 10,000+ cryptocurrencies
- **Market data**: Market cap, volume, circulating supply, ATH/ATL
- **Trending coins**: What's moving in the market
- **Exchange data**: Trading pairs and volume across exchanges

```text
    ┌────────────────────────────────────────────────────────────────────┐
    │                                                                    │
    │   🦎 COINGECKO PRO ENDPOINTS VIA SERENAI                           │
    │                                                                    │
    │   ⭐ PAID-ONLY FEATURES (normally $129+/month)                     │
    │   ┌────────────────┐  ┌────────────────┐  ┌────────────────┐      │
    │   │ 🚀 TOP MOVERS  │  │  🆕 NEW COINS  │  │  📊 OHLC RANGE │      │
    │   │ Scan 10,000+   │  │  Latest 200    │  │  Custom date   │      │
    │   │ coins at once  │  │  listings      │  │  candlesticks  │      │
    │   └────────────────┘  └────────────────┘  └────────────────┘      │
    │                                                                    │
    │   STANDARD ENDPOINTS                                               │
    │   ┌────────────────┐  ┌────────────────┐  ┌────────────────┐      │
    │   │  💵 PRICES     │  │  📈 MARKETS    │  │  🏦 EXCHANGES  │      │
    │   │  Real-time &   │  │  Market cap    │  │  Trading pairs │      │
    │   │  historical    │  │  Volume, ATH   │  │  & volume      │      │
    │   └────────────────┘  └────────────────┘  └────────────────┘      │
    │                                                                    │
    │   50+ endpoints  •  Pro API access  •  $0.0005/call               │
    │                                                                    │
    └────────────────────────────────────────────────────────────────────┘
```

## Get Started in 3 Prompts

**Prompt 1 - Fund your wallet:**
> "Explain how to fund an Ethereum wallet with USDC on Base network using Coinbase"

**Prompt 2 - Test the connection (paid-only endpoint):**
> "Use SerenAI to fetch the 5 most recently listed coins from CoinGecko's /coins/list/new endpoint and confirm the payment went through"

**Prompt 3 - Build something (paid-only endpoint):**
> "Create a market-wide momentum scanner using CoinGecko's /coins/top_gainers_losers endpoint to find the top 5 coins with >10% gains and market cap over $50M, then store the results in SerenDB"

No API keys to manage. No subscriptions to cancel. No vendor lock-in. Just data when you need it, paid for when you use it.

The future of AI agent infrastructure is pay-per-use. CoinGecko on SerenAI is proof it works.

```text
    ╔════════════════════════════════════════════════════════════════════╗
    ║                                                                    ║
    ║   🚀  G E T   S T A R T E D   I N   3   P R O M P T S             ║
    ║                                                                    ║
    ║   ┌──────────────────────────────────────────────────────────┐    ║
    ║   │  PROMPT 1: Fund your wallet                              │    ║
    ║   │  "Explain how to fund an Ethereum wallet with USDC..."   │    ║
    ║   └──────────────────────────────────────────────────────────┘    ║
    ║                          ↓                                        ║
    ║   ┌──────────────────────────────────────────────────────────┐    ║
    ║   │  PROMPT 2: Test with paid-only endpoint ⭐                │    ║
    ║   │  "Fetch the 5 most recently listed coins..."             │    ║
    ║   └──────────────────────────────────────────────────────────┘    ║
    ║                          ↓                                        ║
    ║   ┌──────────────────────────────────────────────────────────┐    ║
    ║   │  PROMPT 3: Build with paid-only data ⭐                   │    ║
    ║   │  "Market-wide scanner via /top_gainers_losers..."        │    ║
    ║   └──────────────────────────────────────────────────────────┘    ║
    ║                                                                    ║
    ║   ⭐ = PAID-ONLY endpoints (data unavailable on free API)        ║
    ║   ✅ No API keys  ✅ No subscriptions  ✅ No vendor lock-in       ║
    ║                                                                    ║
    ╚════════════════════════════════════════════════════════════════════╝
```

---

## About SerenAI

SerenAI is building the payment infrastructure for the AI agent economy. Our x402 Gateway enables AI agents to autonomously pay for data using USDC micropayments on Base—no subscriptions, no API keys, no human intervention.

We believe AI agents will pay for millions of dollars in data daily. We're creating the payment rails to make it happen.

**Our Stack:** TypeScript, PostgreSQL, Base (Ethereum L2), Coinbase x402 Protocol

**Current Data Publishers:** CoinGecko, Nasdaq Data Link, U.S. Treasury, Polymarket, Google Trends

---

> **🚀 Ready to build?** Sign up at **[console.serendb.com/signup](https://console.serendb.com/signup)** with invite code **`serenai2025`**

*Questions? Email hello@serendb.com or join our [Discord](https://discord.gg/serenai).*

**🦎 CoinGecko on SerenDB:** [serendb.com/bestsellers/a42f2c7f-c075-488f-a3f9-acfd7304934b](https://serendb.com/bestsellers/a42f2c7f-c075-488f-a3f9-acfd7304934b)
**Publisher ID:** `a42f2c7f-c075-488f-a3f9-acfd7304934b`
**Price:** $0.0005 per call (0.05 cents)
**Network:** Base Mainnet (USDC)

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
║     │  TypeScript  •  PostgreSQL  •  Base (Ethereum L2)  •  x402      │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     DATA PUBLISHERS:                                                         ║
║     🦎 CoinGecko  •  📊 Nasdaq  •  🏛️ Treasury  •  🎯 Polymarket  •  🔍 Google║
║                                                                              ║
║                      hello@serendb.com | discord.gg/serenai                  ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```