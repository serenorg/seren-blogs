# How-to Use Seren Polymarket Predictions to Increase Your Trading Edge

```
 ____  _____ ____  _____ _   _
/ ___|| ____|  _ \| ____| \ | |  x
\___ \|  _| | |_) |  _| |  \| | x x   POLYMARKET
 ___) | |___|  _ <| |___| |\  |x   x  PREDICTIONS
|____/|_____|_| \_\_____|_| \_| x x
                                 x
    $$$  FIND THE EDGE  $$$
  +---------+---------+---------+
  | KALSHI  |MANIFOLD |METACULUS|
  |  0.62   |  0.58   |  0.65  |
  +---------+---------+---------+
        \       |       /
         v      v      v
      [  CONSENSUS: 0.617  ]
      [ POLYMARKET: 0.55   ]
      [  DIVERGENCE: +67bp ]  <-- EDGE
```

---

> **What does it cost?**
> - **Backtesting:** Free (uses public Polymarket CLOB data + your [SerenBucks](https://serendb.com) for market discovery)
> - **Predictions intelligence:** ~$0.30 per backtest run (batch consensus $0.15 + batch divergence $0.15)
> - **Per-market live signals:** $0.05 per market (consensus, volatility, or orderbook)
> - **Tracking 6 platforms by hand:** ~4 hours/day. Seren does it in 38ms for $0.05.
>
> Get [SerenBucks](https://serendb.com) to start. No subscriptions. Pay only for what you use.

---

## The Three Big Problems Every Polymarket Maker Faces

Imagine you have a lemonade stand. You want to sell lemonade AND buy lemonade from others. That is what a "maker" does on [Polymarket](https://serendb.com/skills) -- you put up prices to buy and sell, and you earn a little bit each time someone trades with you.

But here are three big problems:

**Problem 1: You are trading blind.** You only see what Polymarket shows you. You do not know what people on Kalshi, Manifold, Metaculus, PredictIt, or Betfair think about the same question. That is like selling lemonade without knowing the price at every other lemonade stand on the block.

**Problem 2: You pick bad markets.** There are hundreds of markets on Polymarket. Some are busy with lots of buyers and sellers. Some are empty. If you pick the wrong ones, you sit around all day with no trades. Without a smart way to score and rank markets, you are guessing.

**Problem 3: You do not know which way the price should go.** When you make a market, you set a bid price and an ask price. If the real price is higher than what Polymarket shows, you want to lean your bid up. If it is lower, you lean your ask down. But without outside data, you have no idea which way to lean. You are flipping a coin.

---

## How Other Betting Markets Give You an Edge

```
  OTHER MARKETS KNOW THINGS YOU DON'T:

  KALSHI ------> "0.62 YES"
  MANIFOLD ----> "0.58 YES"
  METACULUS ---> "0.65 YES"
  PREDICTIT --> "0.60 YES"
  BETFAIR ----> "0.63 YES"
                   |
                   v
            AVG = 0.616

  POLYMARKET -> "0.55 YES"  (CHEAP!)

  YOU: "I should BUY on Polymarket!"
       "It's 6.6 cents too low!"
```

Every day, thousands of traders bet on the same events across six or more prediction platforms. Each platform has its own crowd, its own information, and its own price. When all those prices agree but Polymarket is different, that gap is your edge.

The hard part? Tracking five or six platforms in real-time is exhausting. Prices change every second. Matching "Will Russia and Ukraine reach a ceasefire?" on Polymarket to the equivalent question on Kalshi (which might word it differently) requires fuzzy matching and embeddings. Doing this by hand for 300 markets? Impossible. You need a machine to do it for you, continuously, and serve you the answer instantly.

---

## Seren Polymarket Predictions: Your Real-Time Intelligence Feed

[Seren's](https://serendb.com) new publisher -- **Seren Polymarket Predictions** -- does exactly this. It aggregates prices from Kalshi (13,800+ markets), Manifold (3,000+ markets), Metaculus, PredictIt, and Betfair, matches them to Polymarket questions using embedding similarity, and serves you consensus pricing, divergence signals, and actionable trade recommendations through simple API calls.

Here are the endpoints and what they cost:

| Endpoint | What It Does | Cost |
|----------|-------------|------|
| `GET /api/oracle/consensus/{id}` | Cross-platform consensus probability for one market | $0.05 |
| `POST /api/oracle/consensus/batch` | Batch consensus for up to 50 markets at once | $0.15 |
| `GET /api/oracle/divergence` | Find markets where Polymarket diverges from consensus | $0.05 |
| `POST /api/oracle/divergence/batch` | Batch divergence analysis for multiple markets | $0.15 |
| `POST /api/oracle/actionable` | Actionable signals with trade direction and edge size | $0.15 |
| `GET /api/oracle/match` | Find cross-platform matches for a market question | $0.05 |
| `GET /api/polymarket/correlations` | Correlated market pairs with basis stats | $0.10 |
| `GET /api/polymarket/pairs/suggested` | Suggested pairs trades ranked by deviation | $0.10 |
| `GET /api/polymarket/orderbook/{id}` | Real-time order book with spread and depth | $0.05 |
| `GET /api/polymarket/volatility/{id}` | Multi-window volatility with regime classification | $0.05 |
| `GET /api/oracle/categories` | List canonical event categories | Free |
| `GET /api/oracle/platforms/{platform}/markets` | List tracked markets on any platform | Free |

A full backtest with intelligence costs about **$0.30 in [SerenBucks](https://serendb.com)** -- one batch consensus call plus one batch divergence call. That is less than a penny per market.

---

## Get Your Predictions Edge in 90 Seconds

```
  STEP 1        STEP 2        STEP 3
 +------+     +------+     +------+
 |INSTALL|     | SIGN |     | KEYS |
 | SEREN |---->|  UP  |---->| .env |
 |DESKTOP|     |      |     |      |
 +------+     +------+     +------+

  STEP 4        STEP 5
 +------+     +--------+
 |LAUNCH |     |ACTIVATE|   --> $$$
 | AGENT |---->| SKILL  |   BACKTEST!
 |       |     |        |   +90% !!!
 +------+     +--------+
```

**Step 1: Install SerenDesktop.** Go to [serendb.com/install](https://serendb.com/install) and download SerenDesktop for your operating system. It takes about 30 seconds.

**Step 2: Register for Seren.** Head to [serendb.com/sign-up](https://serendb.com/sign-up) and create your account. Make sure your password is at least 11 characters long.

**Step 3: Set up your Polymarket keys.** Collect your Polymarket API key, API secret, passphrase, and wallet private key. Put them in your `.env` file. Make sure you have at least **$100 of bankroll** in your Polymarket wallet. If you need to convert SerenBucks into Polymarket USDC, send an email to [hello@serendb.com](mailto:hello@serendb.com) with your request and we will email you instructions.

**Step 4: Launch an agent.** Start SerenDesktop and launch an AI agent. You can use your Claude or Codex subscription, or you can use [SerenBucks](https://serendb.com) to pay for any AI model you wish to trade with.

**Step 5: Activate the skill.** Search for the **Polymarket Maker Rebate Bot Skill** in the [skills panel](https://serendb.com/skills), click to save, then click to activate. Your AI agent will see the skill activating and automatically run a backtest with current market data to show you possible historical returns.

---

## Backtest Results: What We Saw

We backtested the Polymarket maker-rebate-bot over 90 days with a $100 bankroll across 20 markets selected from a pool of 300 candidates. Market selection uses a composite scoring algorithm that ranks candidates by market-making suitability: price proximity to 0.50 (maximizing two-way order flow), 24-hour trading volume (ensuring fill opportunities), and bid-ask spread tightness (favoring liquid, competitive markets). All 300 candidates are evaluated concurrently -- history is fetched from the Polymarket CLOB `/prices-history` endpoint and orderbooks are synthetically reconstructed from live book snapshots. The top 20 scored markets receive equal capital allocation ($5 each), with per-market bankruptcy halts and equity floors to prevent unrealistic negative returns.

On this baseline run, the bot achieved a **+90% return** ($100 to $190), generating 2,522 fill events across $2,974 in filled notional with a maximum drawdown of $113.

**A real example from the backtest:** The market "Will Michael B. Jordan win Best Actor at the 98th Academy Awards?" scored high on our composite ranking -- the price sat near 0.50 (maximum two-way flow), volume was strong, and the spread was tight. With just $5 of allocated capital, the bot generated 133 fill events across $109 in filled notional and earned **+$158.69 in profit** -- a 3,174% return on that single market's allocation. Meanwhile, the market "Will Israel launch a major ground offensive in Lebanon by March 31?" contributed **+$64.92** from only 26 fills. These are the kinds of markets that a scoring algorithm surfaces and a blind picker misses.

The next phase integrates **Seren Polymarket Predictions intelligence** -- cross-platform consensus pricing and divergence signals from Kalshi, Manifold, Metaculus, PredictIt, and Betfair -- to further improve market selection and directional quote skew. We expect the intelligence-enhanced backtest to show measurable improvement in both return and drawdowns when your agent runs the Polymarket skill.

---

## Activate Live Trading and Track Your PnL

Once your backtest looks good, flip `dry_run` to `false` and `live_mode` to `true` in your config. Your agent will begin quoting real orders on Polymarket.

All your PnL, fill events, and market telemetry are tracked in [SerenDB](https://serendb.com) so you can review performance at any time. For ongoing monitoring and automatic adjustments, use **seren-cron** to schedule your agent on a recurring interval -- it will re-evaluate markets, refresh predictions signals, and adjust quotes without you lifting a finger.

Every [Seren Skill](https://serendb.com/skills) is fully open source. Fork the repo, tweak the scoring algorithm, adjust the spread parameters, or add your own signals. Your AI agent can modify the code directly and redeploy. The infrastructure serves you -- not the other way around.

---

## Conclusion: Do Not Trade Without an Edge

```
  WITHOUT SEREN:          WITH SEREN:

  "Pick 20 random       "Score 300 markets,
   markets and hope"     pick the best 20,
                          add predictions edge"
      |                       |
      v                       v
   -56% LOSS              +90% GAIN

  Your bankroll:          Your bankroll:
  $100 --> $44            $100 --> $190

  (  ;_;  )               ( ^_^ )  $$$
```

Trading on Polymarket without an edge is gambling. Our own baseline tests showed **-56% returns** when markets were selected arbitrarily. That means your $100 becomes $44 in 90 days. With [Seren's](https://serendb.com) scored market selection alone, that same $100 became $190. Add cross-platform predictions intelligence and you get a directional edge that no single-platform trader can match.

The data is there. The infrastructure is there. The [skills are free and open source](https://serendb.com/skills). The only question is whether you want to trade with an edge -- or without one.

Get started at [serendb.com/install](https://serendb.com/install).

---

## Disclaimers

**Trading Disclaimer:** These skills are software tools for strategy research and execution support. They are not financial, investment, legal, or tax advice. Trading involves substantial risk, including total loss of capital. Backtests are hypothetical, rely on assumptions, and do not guarantee future results. Always test in dry-run mode first, monitor behavior continuously, and deploy only risk capital you can afford to lose.

**Prediction Market Disclaimer:** Prediction market participation may be regulated, restricted, or prohibited depending on jurisdiction. You are responsible for complying with local laws, platform terms, age requirements, licensing rules, and reporting obligations. Do not use these skills to bypass geographic restrictions or policy controls. Confirm legal eligibility before any live trading activity.

---

## About SerenAI

SerenAI is building payment infrastructure for the AI agent economy. Our x402 Gateway enables AI agents to autonomously pay for data and services using USDC micropayments on Base -- no subscriptions, no API keys, no human intervention.

**Our Stack:** TypeScript, Rust, Python, PostgreSQL, Cloudflare Workers, Base (Ethereum L2), Coinbase x402 Protocol

**Data Publishers:** Google, Nasdaq, Coinbase, Kraken, Alpaca, CoinGecko, Firecrawl, Perplexity, OpenAI, Moonshot AI, CoinMarketCap, and 110+ more

**Contact us:** [serendb.com](https://serendb.com) | [hello@serendb.com](mailto:hello@serendb.com)
