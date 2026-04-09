# Introducing Seren Polymarket Strategy Skills

**Published**: March 5, 2026  
**Reading Time**: 8 minutes  
**Author**: Taariq Lewis, SerenAI

```
+--------------------------------------------------------------------------------+
|   ____       _       _   _               __  __             _         _       |
|  / ___| ___ | |_ ___| |_(_)_ __   __ _  |  \/  | __ _ _ __| | _____| |_     |
| | |  _ / _ \| __/ __| __| | '_ \ / _` | | |\/| |/ _` | '__| |/ / _ \ __|    |
| | |_| | (_) | |_\__ \ |_| | | | | (_| | | |  | | (_| | |  |   <  __/ |_     |
|  \____|\___/ \__|___/\__|_|_| |_|\__, | |_|  |_|\__,_|_|  |_|\_\___|\__|    |
|                                   |___/                                        |
|                SIGNALS | ODDS | EXECUTION | RISK GUARDS                       |
+--------------------------------------------------------------------------------+
```

Today we are announcing a full set of Polymarket strategy skills in Seren: one autonomous trading bot and four focused strategy modules. The release is built for operators who want repeatable execution, not one-off scripts. Every skill follows the same discipline: dry-run first, replay and quality gates before live intents, and explicit controls that prevent accidental production exposure. If you have been mixing ad hoc notebooks and manual decision loops, these skills give you a cleaner path from research to deployment.

**Polymarket Trading Bot** (`polymarket/bot`) is the broad orchestration skill for teams that want a complete market loop. It scans active markets, enriches candidates with AI research, estimates fair value, and coordinates action decisions under defined constraints. It is less about one specific edge and more about system control: data ingestion, reasoning, ranking, and execution workflow in one skill. If your goal is to run a unified engine that combines `polymarket-data`, `polymarket-trading`, and model-based valuation, this is the right starting point. Use it when you need an extensible core that can host multiple strategy overlays.

**Polymarket Maker Rebate Bot** (`polymarket/maker-rebate-bot`) is optimized for microstructure discipline in binary markets. It focuses on a specific thesis: a maker can capture spread plus rebate while avoiding poor fills through strict inventory controls and market-quality filters. The skill runs a replay-first process, excludes near-resolution markets, and gates quote generation until backtest criteria are satisfied. This is important because maker strategies fail when they ignore adverse selection and unwind costs. The bot explicitly models that tradeoff. For users building systematic quote logic, this skill gives a realistic framework that values survival and risk-adjusted edge over naive fill volume.

```
   .-------------------.        .----------------------.
  /  MAKER REBATE BOT  /|      / spread + rebate -     /|
 /--------------------/ |     / adverse selection      / |
|   BID      ASK      |  |   |------------------------|  |
|  0.47    0.49       |  |   | inventory guard   ON   |  |
|  [buy]   [sell]     |  |   | near-expiry filter ON  |  |
|_____________________| /    |________________________| /
|_____________________|/
```

**Paired Market Basis Maker** (`polymarket/paired-market-basis-maker`) targets relative-value dislocations across logically linked contracts. Instead of predicting direction, it looks for pricing inconsistencies between connected markets and models convergence behavior over historical replay. That structure can reduce outright directional exposure and shift the focus toward spread normalization. The skill includes sample gating and backtest gating before any two-leg intent is emitted, which helps prevent overfitting and low-signal deployment. If you are already comfortable with pair logic in other asset classes, this skill translates that discipline to prediction markets with the controls you need for repeatable operation.

```
        Pair A                            Pair B
   +---------------+                 +---------------+
   | YES contract  |<--- basis --->  | NO contract   |
   | too rich?     |                 | too cheap?    |
   +-------+-------+                 +-------+-------+
           \                                 /
            \_______ convergence trade _____/
                     (hedged two-leg intent)
```

**Liquidity Paired Basis Maker** (`polymarket/liquidity-paired-basis-maker`) narrows the same paired strategy into a liquidity-aware universe. In practice, many attractive pair spreads are not executable at meaningful size once depth and spread instability are considered. This skill filters aggressively so generated intents are more likely to be tradable in live conditions. It uses a shorter replay window than the broader paired maker to stay closer to current market regime while still requiring enough events for reliability. If your priority is execution quality and slippage control over theoretical opportunity count, this variant is often the best first production candidate.

**High-Throughput Paired Basis Maker** (`polymarket/high-throughput-paired-basis-maker`) extends paired logic for wider coverage and faster cycle throughput. It is intended for teams that already validated paired workflows and now want denser opportunity scanning without abandoning guardrails. Higher throughput can improve capture frequency, but only if process discipline remains intact. This skill keeps mandatory backtest-first flow and explicit live confirmations, so scale does not come at the cost of control. Use it when you want to industrialize paired basis operations with stronger automation and broader market sampling while preserving risk boundaries.

```
[scan many markets] -> [pair graph] -> [quality gate] -> [ranked intents]
      2000+ mkts          100+ pairs         strict filter         deploy set
```

Across the release, the main advantage is operational consistency. Each skill follows a recognizable execution contract: replay, gate, then intent. That makes strategy comparison cleaner, incident response faster, and onboarding easier for new operators. It also helps teams run multiple strategies in parallel without reinventing environment controls, logging conventions, and validation rules each time.

If you are deciding where to begin, start with Maker Rebate Bot for microstructure-focused quoting or Liquidity Paired Basis Maker for controlled relative-value execution. Move to High-Throughput Paired Basis Maker only after validating assumptions, fill behavior, and monitoring under live-like conditions. Use Polymarket Trading Bot as the orchestration layer when you want one control plane across research, ranking, and execution.

## Trading Disclaimer

These skills are software tools for strategy research and execution support. They are not financial, investment, legal, or tax advice. Trading involves substantial risk, including total loss of capital. Backtests are hypothetical, rely on assumptions, and do not guarantee future results. Always test in dry-run mode first, monitor behavior continuously, and deploy only risk capital you can afford to lose.

## Prediction Market Disclaimer

Prediction market participation may be regulated, restricted, or prohibited depending on jurisdiction. You are responsible for complying with local laws, platform terms, age requirements, licensing rules, and reporting obligations. Do not use these skills to bypass geographic restrictions or policy controls. Confirm legal eligibility before any live trading activity.

```
+--------------------------------------------------------------------------------+
|                                  S E R E N                                     |
|                      build fast | validate hard | trade responsibly            |
|                                                                                 |
|               [data] -> [models] -> [strategy] -> [execution]                 |
+--------------------------------------------------------------------------------+
```
