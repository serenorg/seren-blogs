# How-To Trade Curve Gauges with AI Agent Trading Skills & x402 Payments on Seren

```text
  ________________________________________________________________________________
 /                                                                                \
|   HOW-TO: TRADE CURVE GAUGES WITH AI AGENT SKILLS + x402 PAYMENTS ON SEREN     |
|                                                                                  |
|     +---------------------+     +---------------------+     +----------------+   |
|     |  Seren Desktop      | --> |  Curve Skill        | --> |  RPC + Curve   |   |
|     |  agent runtime      |     |  paper/live modes   |     |  publishers    |   |
|     +---------------------+     +---------------------+     +----------------+   |
|                                                                                  |
|     Goal: move from "looking at yields" to "controlled execution" fast.         |
 \________________________________________________________________________________/
```

If you already use Curve and want to automate gauge decisions without building a full bot stack, this guide is for you. You will install one skill in Seren Desktop, run a paper-trade flow first, then decide whether to switch to live execution.

This is a practical how-to for operators, not a protocol deep-dive. You need a wallet, a clear risk cap, and a repeatable process.

## What You Are Actually Automating

The Curve Gauge Yield Trader skill does four concrete jobs:

1. Pulls current Curve gauge data from the Curve Finance Seren publisher.
2. Selects a candidate gauge and builds a trade plan.
3. Runs RPC preflight checks per chain before execution.
4. In paper mode, simulates and returns unsigned/signed transaction plans; in live mode, signs and broadcasts from your local environment.

The x402 part matters because the agent is interacting with paid publisher infrastructure through the Seren gateway.

## What You Need Before You Start

- Seren Desktop installed: [https://serendb.com/install](https://serendb.com/install)
- A Seren API key (if your runtime path needs it)
- A funded wallet only when you move to live mode
- A conservative first risk target (for example, $25-$100 notional)

For skill source and implementation details:

- Curve skill repo path: [https://github.com/serenorg/seren-skills/tree/main/curve/curve-gauge-yield-trader](https://github.com/serenorg/seren-skills/tree/main/curve/curve-gauge-yield-trader)
- Full skills catalog: [https://github.com/serenorg/seren-skills](https://github.com/serenorg/seren-skills)

## Fast Start in Seren Desktop

1. Open Seren Desktop and create a new agent.
2. Search for the Curve gauge skill and add it to your agent.
3. Set your chain, wallet mode, and deposit target.
4. Keep `dry_run=true` and `live_mode=false` for first runs.
5. Execute and review the output, especially:
   - selected gauge
   - expected reward/APY field
   - preflight transaction count
   - gas and estimation warnings
6. Iterate until output is stable and understandable.

```text
+----------------------------------------------------------------------------------+
|                              QUICKSTART FLOW                                     |
+----------------------------------------------------------------------------------+
|  Install Seren Desktop -> Create Agent -> Add Curve Skill -> Set Config          |
|          |                   |                    |                 |             |
|          v                   v                    v                 v             |
|      Connect key        Choose chain         wallet_mode=local   dry_run=true    |
|          |                   |                    |                 |             |
|          +-------------------+--------------------+-----------------+             |
|                                      |                                             |
|                                      v                                             |
|                            Run paper-trade preflight                               |
|                                      |                                             |
|                                      v                                             |
|                     Inspect plan, warnings, gas, and guardrails                    |
+----------------------------------------------------------------------------------+
```

## Paper Trading: The Right First Milestone

Paper trading here means you run the complete decision and preflight path without sending real transactions.

For example, a $100 paper run on Base should give you:

- `status=ok`
- `mode=dry-run`
- a resolved RPC publisher (for Base)
- one or more prepared transactions in preflight

If any of those are missing, do not switch to live mode yet.

The most useful paper-trade output is not the top APY number. It is the execution artifact set:

- which gauge was selected
- whether allowance/approval is required
- whether gas estimation succeeded cleanly or fell back
- whether any transaction leg reverts at estimate time

That output tells you if your live run is likely to fail.

## Moving from Paper to Live Safely

Live mode should be a controlled promotion.

Use this sequence:

1. Confirm repeated dry runs produce sane, consistent plan output.
2. Use low notional and explicit bounds.
3. Ensure wallet funding is intentional and isolated.
4. Enable live mode in config.
5. Require explicit live confirmation at run time.
6. Observe first execution and verify on-chain effects.

```text
   PAPER MODE                           LIVE MODE
+------------------+               +-----------------------+
| dry_run=true     |               | dry_run=false         |
| live_mode=false  |               | live_mode=true        |
| no tx broadcast  |               | tx can broadcast      |
| safe iteration   |               | explicit opt-in gate  |
+------------------+               +-----------------------+
          |                                    |
          +------------- promote only after ---+
                        repeatable preflight
```

Local mode signs locally and sends raw transactions through the resolved RPC publisher. Ledger mode signs externally after clean preflight.

## Where x402 Helps in Real Ops

x402-backed access lets agents use paid APIs and execution services without custom billing glue.

In practice that means:

- predictable access path to Curve and chain RPC publishers
- unified payment and request flow through Seren
- easier automation when you schedule recurring checks or runs

You can combine the skill with recurring triggers, but keep autonomous scheduling in paper mode until you have:

- stable preflight quality
- defined stop conditions
- clear escalation rules for failed estimates or RPC errors

## Common Operator Mistakes to Avoid

- Treating high APY as a complete signal.
- Skipping paper runs on each chain change.
- Ignoring estimation warnings in preflight.
- Running live with a wallet that also holds unrelated funds.
- Expanding notional before verifying first live transaction receipts.

Reliable setups stay boring: same chain, small lots, checklist, explicit approval always.

## Practical Checklist You Can Reuse

Before each run:

- chain selected intentionally
- wallet mode and address verified
- dry vs live state confirmed
- target amount and token validated

After each run:

- trade plan looks plausible
- transaction count matches expectation
- gas figures are within expected range
- warnings reviewed and documented

Before enabling automation:

- at least several clean paper runs
- at least one tiny live run verified on-chain
- rollback plan defined (pause schedule, disable live mode, reduce notional)

This process turns an AI trading skill from a demo into an operator tool.

## Final Take

If you still trade manually, this skill gives leverage: install, paper trade, inspect outputs, then promote.

The edge is disciplined, repeatable execution and clear visibility into what the agent is doing.

```text
 __________________________________________________________________________________
|  NEXT STEP: START HERE                                                            |
|                                                                                   |
|   [1] Explore SerenDB + agents:  https://serendb.com                             |
|   [2] Install Seren Desktop:     https://serendb.com/install                     |
|                                                                            |
|      ____                          ____                                     |
|     / __ \   TRADE WITH CONTROLS  / __ \    PROMOTE PAPER -> LIVE          |
|    / /  \_\  NOT GUESSWORK       / /  \_\   WITH EXPLICIT CHECKS           |
|   | |                            | |                                       |
|   | |  SEREN + SKILLS + x402     | |  OPERATOR-FIRST FLOW                 |
|    \ \__/ /                      \ \__/ /                                   |
|     \____/                        \____/                                    |
|__________________________________________________________________________________|
```

## Get Started

- What is Seren? [https://serendb.com](https://serendb.com)
- Full API Docs: [https://docs.serendb.com](https://docs.serendb.com)
- Sign Up Free: [https://console.serendb.com](https://console.serendb.com)
- Download Seren Desktop in 90 seconds or less: [https://serendb.com/install](https://serendb.com/install)
- Questions? Drop them in comments or join our Discord: [https://discord.gg/jseg7q4KS7](https://discord.gg/jseg7q4KS7)

## Legal, Tax, and Accounting Disclaimers

- This post is for informational and educational purposes only.
- Nothing in this post is legal, tax, accounting, investment, or financial advice.
- You are solely responsible for your filings, positions, classifications, and jurisdiction-specific compliance.
- Tax treatment for digital assets varies by jurisdiction and may change over time.
- AI-generated analysis may contain errors, hallucinations or incomplete assumptions.
- Always review outputs and consult a licensed CPA, EA, tax attorney, or other qualified professional before filing.
- No warranty is provided for completeness, accuracy, fitness for purpose, or audit outcomes.

Tags: #curve #defi #trading #agents #serendb #serendesktop #x402 #automation #crypto
