# Seren Skills Updates: Polymarket Trading Skills Get Safer, Smarter, and Scheduled

**Published**: March 16, 2026
**Reading Time**: 5 minutes
**Author**: Taariq Lewis, SerenAI

```
+================================================================================+
|                                                                                |
|     ____  _____ ____  _____ _   _    ____  _  ___ _     _     ____             |
|    / ___|| ____|  _ \| ____| \ | |  / ___|| |/ (_) |   | |   / ___|            |
|    \___ \|  _| | |_) |  _| |  \| |  \___ \| ' /| | |   | |   \___ \            |
|     ___) | |___|  _ <| |___| |\  |   ___) | . \| | |___| |___ ___) |           |
|    |____/|_____|_| \_\_____|_| \_|  |____/|_|\_\_|_____|_____|____/            |
|                                                                                |
|          P O L Y M A R K E T   S K I L L S   U P G R A D E                    |
|                    ~ March 2026 Edition ~                                      |
|                                                                                |
|         BEFORE                               AFTER                             |
|     +-----------+                       +-----------+                          |
|     |  BOT v1   |   ~~~>  UPGRADE  ~~~> |  BOT v2   |                          |
|     |  :/  oops |   ~~~>   ~~~>    ~~~> |  B)  safe |                          |
|     | -trading- |                       | -trading- |                          |
|     |  no cron  |                       | +cron +$$ |                          |
|     |  bad math |                       | +backtest |                          |
|     +-----------+                       +-----------+                          |
|           |                                   |                                |
|      [hope for                          [fail closed,                          |
|       the best]                          protect capital]                      |
|                                                                                |
+================================================================================+
```

## Introduction: Seren, Seren Skills, and the Polymarket Trading Suite

Seren is an agentic platform that gives AI agents the tools they need to execute real work — from database queries and web scraping to live trading on prediction markets. At the center of that execution layer are Seren Skills: modular, installable instruction sets that teach your AI agent how to perform specific workflows end-to-end. Skills handle everything from configuration and dry-run validation to live execution and monitoring, so you get repeatable operations instead of ad hoc prompting. The Polymarket skill family is the most developed trading suite in the Seren ecosystem. It includes five skills — the main Polymarket Trading Bot, the Maker Rebate Bot, the Paired Market Basis Maker, the Liquidity Paired Basis Maker, and the High-Throughput Paired Basis Maker — each targeting a different strategy profile from directional fair-value trading to microstructure quoting to relative-value pair arbitrage. Over the last week, every one of these skills received significant upgrades focused on safety, scheduling, and backtest accuracy. Here is what changed and why it matters for Polymarket traders.

## Most Important Upgrades: Fail-Closed Safety and Seren-Cron Scheduling

```
+--------------------------------------------------------------------------------+
|                                                                                |
|   FAIL-CLOSED: How your bot handles trouble now                                |
|                                                                                |
|          ,--------.                                                            |
|         /  MARKET  \          ERROR DETECTED!                                  |
|        |  volatility |        +-----------+                                    |
|        |   spike!!   |------->| HALT BOT  |                                    |
|         \   O_O    /          | SAVE STATE|                                    |
|          `--------'           | LOG FAULT |                                    |
|               |               +-----------+                                    |
|               v                     |                                          |
|     OLD WAY: keep trading           v                                          |
|     and pray  (._.)         NEW WAY: capital protected                         |
|     lost $$$$ overnight      trader sleeps soundly  (-_-)zzZ                   |
|                                                                                |
|   SEREN-CRON: Your bot runs on a schedule, not a prayer                        |
|                                                                                |
|     OLD: $ nohup python agent.py &    NEW: "Hey Claude, schedule my bot"       |
|          [invisible daemon lurking]        [managed cron via Seren MCP]        |
|          [forgot it was running...]        [auto-pause if funds low]           |
|          [3 days later: rekt]              [pause / resume / delete]           |
|                                                                                |
|          ___                                  ___                              |
|         | $ |  <- your money                 | $ |  <- your money              |
|         | $ |     slowly                     | $ |     still                   |
|         |   |     vanishing                  | $ |     there                   |
|         |___|                                |_$$|                              |
|                                                                                |
+--------------------------------------------------------------------------------+
```

The single most consequential change this week is the introduction of **fail-closed live trading fault handling** across the entire Polymarket suite (PR #121). Previously, if a live trading agent encountered an unexpected error — a malformed API response, a network timeout, a position tracking inconsistency — the behavior was undefined. The agent might retry, skip the error, or continue trading in a degraded state. Now, every Polymarket skill fails closed: any unhandled fault during live execution immediately halts trading, preserves the current position state, and logs the fault for review. This is the difference between a trading system and a script. Scripts break silently. Trading systems fail loudly and protect capital. The same fail-closed pattern was also applied to Kraken, Coinbase, and Alpaca skills (PRs #122, #123), making this a platform-wide safety upgrade.

The second critical upgrade is **Seren-Cron integration across all 14 trading and investment skills** (PR #117). This one solves a real problem that affected users in production. Before this change, when users asked their AI agent to keep a trading bot running after closing the terminal, Claude would suggest using `nohup` or `launchd` — OS-level daemon tools that make permanent, invisible changes to the user's computer without explicit consent. That is unacceptable for a trading agent. The fix replaces all OS daemon suggestions with the `seren-cron` publisher, a managed scheduling service that runs through the Seren MCP. Each skill now includes a standardized five-step cron integration section: health-check the publisher, check for already-running jobs (preventing forgotten live trades), start a local trigger server, create the cron schedule, and manage it through pause, resume, and delete commands. There is also a built-in insufficient-funds guard that auto-pauses the cron when your SerenBucks balance drops too low. No permanent changes to your computer. No invisible background processes. Full visibility and control through the same agent interface you already use.

## Second Most Important Upgrades: Backtest Accuracy and Wallet Balance Fixes

```
+--------------------------------------------------------------------------------+
|                                                                                |
|   BACKTEST FIXES: Your equity curve no longer lies to you                      |
|                                                                                |
|   BEFORE (bankroll $100):          AFTER (bankroll $100):                      |
|                                                                                |
|   $100 |____                       $100 |____                                  |
|    $50 |    \___                    $50 |    \___                               |
|     $0 |........\___                 $0 |........\___________  <- FLOOR         |
|   -$25 |           \___                 |                                      |
|   -$50 |              X  <- WHAT??      "You can't lose more                   |
|         Max DD: $150 on $100             than you started with."               |
|         That's... not how                Max DD: $100. Honest math.            |
|         money works.                                                           |
|                                                                                |
|   SERENBUCKS WALLET: Finally knows your actual balance                         |
|                                                                                |
|   BEFORE:                          AFTER:                                      |
|   +------------------+             +------------------+                        |
|   | SerenBucks: $0.00|             | SerenBucks:$22.77|                        |
|   | (always. every   |             | (real balance,   |                        |
|   |  time. broken.)  |             |  real endpoint,  |                        |
|   +------------------+             |  real parsing)   |                        |
|   Bug 1: wrong URL                 +------------------+                        |
|   Bug 2: wrong envelope             All 23 skills now                          |
|   Bug 3: "$" broke float()          check before trading                      |
|                                                                                |
+--------------------------------------------------------------------------------+
```

The backtest engine received three targeted fixes that collectively make strategy validation far more reliable. First, **max drawdown is now capped at bankroll** across all trading bots (PR #108). This sounds obvious, but equity curves could previously go negative when cumulative losses exceeded the starting bankroll — reporting a max drawdown of $125 on a $100 bankroll. That is physically impossible. A trader cannot lose more than their bankroll. The fix clamps equity curves at zero and caps drawdown statistics at the initial bankroll value, giving you realistic risk metrics that match how real capital actually works. Second, **backtest candidate scoring and ranking by market-making quality** was fixed (PR #95), ensuring the best opportunities surface to the top of your evaluation pipeline instead of being buried by noisy candidates. Third, **CLOB prices-history queries now use `interval=max`** (PR #93), pulling the full available price history instead of truncated windows that distorted backtest replay.

On the infrastructure side, the **SerenBucks wallet balance check** was completely rebuilt. The original implementation always returned $0.00 because it pointed at the wrong API endpoint, failed to unwrap the response envelope, and choked on dollar-sign prefixes in the balance string (PRs #106, #112). All three issues are now fixed, and the working balance check has been rolled out to all 18 non-Polymarket skills as well (PR #109). This means every trading skill in the Seren ecosystem can now accurately verify your SerenBucks balance before executing paid operations — no more silent failures or surprise insufficient-funds errors mid-trade. Additional fixes include removing placeholder market IDs that broke backtests (PR #119), migrating a deprecated maker backtest history URL (PR #115), unwrapping the Seren gateway response envelope correctly (PRs #87, #101), removing the dead `polymarket-trading-serenai` publisher reference (PR #89), fixing quick-start paths to point at canonical skill runtimes (PR #102), and injecting held positions for accurate SELL order generation (PR #84). The Seren Predictions intelligence layer was also wired into the Maker Rebate Bot backtest (PR #97), and users are now prompted to enable Seren Predictions after completing a backtest (PR #104), connecting the AI research pipeline directly into the strategy validation workflow.

## How to Install SerenDesktop and Refresh Your Skills

```
+--------------------------------------------------------------------------------+
|                                                                                |
|   UPGRADE IN 3 STEPS (seriously, that's it)                                    |
|                                                                                |
|        .------------------------.                                              |
|       /  S E R E N  D E S K T O P\                                             |
|      |  ________________________  |                                            |
|      | |                        | |                                            |
|      | |  > install serendb.com | |                                            |
|      | |    [##########] 100%   | |                                            |
|      | |                        | |                                            |
|      | |  STEP 1: Install       | |                                            |
|      | |    serendb.com/install  | |                                            |
|      | |                        | |                                            |
|      | |  STEP 2: Open Claude   | |                                            |
|      | |    "update my skills"  | |                                            |
|      | |                        | |                                            |
|      | |  STEP 3: Trade         | |                                            |
|      | |    B) you're upgraded  | |                                            |
|      | |________________________| |                                            |
|      |  _  _ _  _ _  _ _  _ _    |                                             |
|       \__________________________/                                             |
|                  |    |                                                         |
|                  |____|                                                         |
|                                                                                |
|   YOU:  "Hey Claude, refresh my Polymarket skills"                             |
|   BOT:   Pulling latest SKILL.md definitions...                                |
|          Updating fail-closed handlers...                                       |
|          Wiring seren-cron integration...                                       |
|          Done. All 5 Polymarket skills upgraded.                                |
|   YOU:  "That's it?"                                                           |
|   BOT:  B)  That's it.                                                         |
|                                                                                |
+--------------------------------------------------------------------------------+
```

To get these upgrades, install SerenDesktop from [serendb.com](https://serendb.com). SerenDesktop connects your AI coding agent — Claude Code, Cursor, Windsurf, or any MCP-compatible client — to the full Seren ecosystem including publishers, skills, and managed scheduling. Once installed, refreshing your skills is simple: just ask your agent. Tell Claude "update my Seren skills" or "refresh the Polymarket skills to the latest version," and your agent will pull the newest SKILL.md definitions and runtime code from the seren-skills repository. There is no manual file management, no git commands to memorize, and no configuration files to edit. Your agent handles the entire update lifecycle. If you are running trading bots on older skill versions, refreshing is especially important this week — the fail-closed safety upgrades and cron integration changes are not backwards-compatible improvements that trickle down automatically. They require the updated skill definitions to take effect. Refresh once, and every future trading session benefits from the new safety and scheduling infrastructure.

## Conclusion: Why These Upgrades Matter for Polymarket Traders

Polymarket is a fast-moving, real-money prediction market where execution discipline separates profitable operators from expensive hobbyists. The upgrades shipped this week address the three failure modes that hurt traders most: silent errors during live execution, unreliable backtests that give false confidence, and fragile scheduling that leaves bots running unmonitored or dying silently. Fail-closed fault handling means your agent protects your capital first and asks questions second. Accurate drawdown capping and improved candidate scoring mean the backtests you run before going live actually reflect the risk you are taking. Seren-Cron integration means your trading agent runs on a managed schedule with visibility, guards, and clean shutdown — not a `nohup` process you forgot about three days ago. Together, these changes move the Polymarket skill suite from a capable but rough research tool toward a production-grade trading operations platform. If you are running Polymarket strategies through Seren, update your skills today. If you have not started yet, there has never been a better week to begin: the safety floor is higher, the backtests are honest, and the scheduling actually works.

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

```
+================================================================================+
|                                                                                |
|                           S  E  R  E  N  A  I                                  |
|                                                                                |
|              "Agents that trade. Infrastructure that pays."                     |
|                                                                                |
|         [SKILLS]---->[BACKTEST]---->[VALIDATE]---->[EXECUTE]                   |
|            |              |              |              |                       |
|            v              v              v              v                       |
|         install        dry-run       fail-closed     seren-cron                |
|         in 10s         first         always          scheduled                 |
|                                                                                |
|                 $$$   built for traders   $$$                                   |
|                                                                                |
|           ___          ___          ___          ___                            |
|          /   \  --->  /   \  --->  /   \  --->  /   \                           |
|         | dat |      | mod |      | str |      | exe |                          |
|          \___/        \___/        \___/        \___/                           |
|          data        models      strategy    execution                          |
|                                                                                |
|         serendb.com  |  hello@serendb.com  |  @seaborneai                      |
|                                                                                |
+================================================================================+
```
