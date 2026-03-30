# How-To use Seren’s Upgraded Polymarket Trading Bot to find Trading Edge of 10% or more

**Published**: March 30, 2026  
**Reading Time**: 6 minutes  
**Author**: Taariq Lewis, SerenAI

```text
+----------------------------------------------------------------------------------+
|   ____  ____  _     __   ___  __  __    _    ____  _  _______ _____             |
|  / ___||  _ \| |    \ \ / / |/ / |  \/  |  / \  _ \| |/ / ____|_   _|            |
|  \___ \| |_) | |     \ V /| ' /  | |\/| | / _ \| |_) | ' /|  _|   | |            |
|   ___) |  __/| |___   | | | . \  | |  | |/ ___ \  _ <| . \| |___  | |            |
|  |____/|_|   |_____|  |_| |_|\_\ |_|  |_/_/   \_\_| \_\_|\_\_____| |_|            |
|                                                                                  |
|      POLYMARKET BOT UPGRADE: SCAN -> SCORE -> RESEARCH -> SIZE -> EXECUTE       |
|                                                                                  |
|      [Gamma] [CLOB] [Perplexity] [Claude] [Kelly] [SerenDB] [seren-cron]        |
+----------------------------------------------------------------------------------+
```

Since our first Polymarket bot video on **February 19, 2026**, the bot has moved from an ambitious prototype into a much more disciplined trading system. The original version had the right shape: scan markets, research them, estimate fair value, size positions with Kelly, and run in dry mode before risking money. The March 30 version tightens the weak points that usually kill prediction market bots in practice: stale prices, duplicate markets, bad ranking, thin books, and sloppy sizing. The upgraded [Polymarket Trading Bot](https://github.com/serenorg/seren-skills/tree/main/polymarket/bot) now addresses those problems directly.

The first major change is the **scan pipeline itself**. Instead of pulling a flat list and hoping the best markets float to the top, the bot now works through a staged funnel: fetch up to 300 live markets, reduce to a candidate pool, and only send the highest-quality slice into AI analysis. That funnel is backed by better ranking, deduplication, and filters. Stale 50/50 Gamma-seeded markets get filtered out early. Markets below a minimum volume floor get dropped. Markets that resolve too far out are excluded. Repetitive event clusters and slug-near-duplicates are capped so the bot does not waste analysis budget on the same theme over and over.

The second big upgrade is **iterative scanning with auto-adjustment**. The bot can now run a small number of iterations and relax or tighten parameters in a controlled way instead of acting like one pass is final truth. That gives it a better chance to surface real candidates when the market is noisy or sparse, while still respecting floors on threshold quality.

The third improvement is **calibration-driven thresholds**. The earlier bot used a hardcoded mispricing threshold. The upgraded bot can now load calibration data and compute an effective threshold based on actual prediction quality. If the model stack is running hot or cold, the edge threshold can reflect that instead of pretending every market deserves the same confidence treatment.

```text
               .-----------------------------------------------.
              /   OLD BOT: "8% edge? good enough."            /|
             /-----------------------------------------------/ |
            |   NEW BOT: "What does calibration say?"        |  |
            |   Brier score     -> check                     |  |
            |   Threshold shift -> adjust                    |  |
            |   Edge quality    -> re-rank                   |  |
            |   Spread quality  -> approve or reject         |  |
            |_______________________________________________| /
            |_______________________________________________|/
```

Then there is the execution layer, which saw some of the most practical fixes. The bot now enforces an **edge-to-spread ratio gate** and **depth-constrained sizing**. That means a model can no longer claim a nice-looking theoretical edge while ignoring that the live spread is wide or the available size is too shallow. The system also got an important CLOB parsing correction: it now reads the **best bid and best ask correctly**, instead of misreading book order in a way that would distort spread calculations.

Market selection got sharper too. The bot now raises the minimum liquidity requirement to **$10,000**, keeps a **minimum volume floor**, rejects extreme divergence cases that look more like broken data than edge, and uses live CLOB price fields when Gamma prices are stale. It also added a fallback research path through `seren-models/sonar` when Perplexity fails.

Another upgrade that is easy to underestimate is output quality and operability. The bot now emits a **structured trade summary block** so each scan is easier to inspect, compare, and debug. It also runs cleanly in backgrounded or piped environments thanks to unbuffered stdout fixes across the trading skills.

On the infrastructure side, the bot is much closer to a real autonomous system. It now supports the **seren-cron local-pull runner** pattern, so the schedule can live in Seren while the actual trading process runs on the machine you control.

## Watch the Video Walkthrough

If you want to see the upgraded setup in action before you run it yourself, watch the new YouTube walkthrough here:

- [How to use SerenDesktop Polymarket Trading Bot](https://youtu.be/ZpV1XLC5ir0)

The video is the fastest way to see the new scan flow, the upgraded execution safeguards, and the SerenDesktop workflow end to end.

## GitHub Repo

The full Polymarket Trading Bot code is open source here:

- [serenorg/seren-skills/tree/main/polymarket/bot](https://github.com/serenorg/seren-skills/tree/main/polymarket/bot)

If you want to inspect the runtime, modify the strategy, or fork the bot for your own deployment, start from that repository.

If you want to try it, the workflow is straightforward:

1. [Download and install SerenDesktop](https://serendb.com/install).
2. [Create your Seren account](https://serendb.com/sign-up).
3. Fund [SerenDB / SerenBucks](https://serendb.com) and configure the Polymarket bot.
4. Start with `--dry-run` and review the structured summaries.
5. Only move to live execution after you are satisfied with the edge quality, spread quality, and position sizing behavior.

The bottom line is simple: the upgraded bot is no longer just “AI reads markets and maybe trades them.” It is now a much stronger pipeline for finding **10%+ edge opportunities** that can survive contact with live market structure. It filters harder, ranks better, adapts thresholds, validates against spread and depth, and produces cleaner operational output.

If you watched the first February 19 video, the difference is clear. That early bot proved the concept. This March 30 version is the one that starts to look like a system you can actually operate: tighter candidate selection, calibration-aware edge gating, fixed CLOB spread math, live-book sanity checks, safer sizing, and production-friendly scheduling.

```text
        ________________________________        ____________________________
       /                                \      /                            \
      |  "EDGE: 12.4%"                   |    |  "SPREAD TOO WIDE"          |
      |  "LIQUIDITY: PASS"               |    |  "DEPTH TOO THIN"           |
      |  "CALIBRATION: OK"               |    |  "REJECT TRADE"             |
      |  "SIZE: $4.80"                   |    |                              |
      |__________________________________|    |______________________________|
                 \                                      /
                  \                                    /
                   \_________ SEREN AI BOT ___________/
                              says "trade less,
                                trade cleaner"
```

## About SerenAI

SerenAI is building the infrastructure layer for agentic software: AI agents that can discover tools, pay for data, run on schedules, persist state, and execute workflows with real operational controls. Seren combines model access, marketplace publishers, scheduling, storage, and payment rails so one agent can move from analysis to action without a pile of brittle glue code. If you want to build trading agents, research agents, or autonomous business workflows, start with [SerenDB](https://serendb.com), install [SerenDesktop](https://serendb.com/install), and create an account at [serendb.com/sign-up](https://serendb.com/sign-up).

## Disclaimers

This post is for informational and educational purposes only. It is not financial, investment, legal, or tax advice. Trading prediction markets involves substantial risk, including the potential loss of all capital. Backtests, scans, rankings, and AI-generated fair value estimates are not guarantees of future performance.

Polymarket access depends on jurisdiction, account eligibility, and current network environment. Prediction market participation may be restricted or prohibited in some regions. You are responsible for complying with local laws, exchange rules, platform terms, tax obligations, and age requirements before attempting any live trading.

Always start in dry-run mode. Review spread, depth, and execution assumptions before risking capital. Never trade money you cannot afford to lose.

```text
+----------------------------------------------------------------------------------+
|    S E R E N A I                                                                 |
|                                                                                  |
|    data -> models -> skills -> payments -> storage -> automation                 |
|                                                                                  |
|    Build agents that do work, not demos.                                         |
|                                                                                  |
|    serendb.com   |   install   |   sign-up   |   skills   |   publishers        |
+----------------------------------------------------------------------------------+
```
