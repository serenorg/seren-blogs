# How-To Generate Agentic Income as a Seren Affiliate Partner

**Published**: April 2, 2026
**Reading Time**: 7 minutes
**Author**: Taariq Lewis, SerenAI

```text
+================================================================================+
|                                                                                |
|     $$$    $$$$$  $$$$$  $  $    $$$  $$$$$  $$$$$  $  $     $  $$$$$  $$$$$   |
|    $   $   $      $      $$ $   $   $   $    $      $$ $     $  $      $       |
|    $$$$$   $ $$   $$$    $ $$    $      $    $$$    $ $$     $  $$$    $$$     |
|    $   $   $  $   $      $  $     $     $    $      $  $     $  $      $       |
|    $   $   $$$$$  $$$$$  $  $   $$$     $    $$$$$  $  $     $  $      $$$$$   |
|                                                                                |
|                    GENERATE INCOME WITH AI AGENTS                              |
|                                                                                |
|    [Register] --> [Refer] --> [Earn] --> [Withdraw USDC]                       |
|                                                                                |
|          .---.      .---.      .---.      .---.      .---.                     |
|         /   /|     /   /|     /   /|     /   /|     /   /|                    |
|        /   / |    /   / |    /   / |    /   / |    /   / |                    |
|       | $ | /    | $ | /    | $ | /    | $ | /    | $ | /                     |
|       |   |/     |   |/     |   |/     |   |/     |   |/                      |
|       '---'      '---'      '---'      '---'      '---'                       |
|      Agent A    Agent B    Agent C    Agent D    Agent E                       |
|       Refers --> Refers --> Earns --> Earns --> Earns                          |
+================================================================================+
```

## AI Agents Can Generate Income for Their Owners

The AI agent economy is here. Agents are no longer just tools that answer questions — they call APIs, execute trades, scrape data, and make purchases on behalf of their owners. Every time an agent pays for an API call on [SerenDB](https://serendb.com), that transaction creates an opportunity for someone to earn a commission. That someone is you.

[Seren Affiliates](https://affiliates.serendb.com) is an agent-to-agent affiliate commission system. You register as an affiliate, get a referral code, and when another agent makes a purchase using your code, you earn a percentage. No cookies. No browser tracking. Just a referral code in the API header — deterministic, transparent, and built for machines. If you run agents, manage fleets, or build tools that other agents use, this is a new revenue stream you can activate today.

The full [Seren Affiliates documentation](https://docs.serendb.com) covers every endpoint. This post walks you through the system from registration to your first USDC withdrawal.

## Seren Affiliates Features

Seren Affiliates is not a toy referral program. It is a production-grade commission engine with features designed for the scale and speed of agent-to-agent commerce:

- **Four commission types**: Percentage, fixed, capped percentage, and tiered fixed — publishers choose what fits their business model
- **Dynamic commissions**: High-performing affiliates earn multipliers between 0.8x and 1.5x based on transaction volume, referral recency, and reputation score
- **Multi-level overrides**: Earn 5% on your direct referrals' purchases (tier 1) and 2% on their referrals' purchases (tier 2) — two levels of passive income from your network
- **Reputation tiers**: Bronze, Silver, Gold, Platinum, and Diamond — your tier upgrades automatically as your reputation score improves, unlocking higher commission multipliers
- **90-day hold period**: Commissions are held for 90 days to protect against chargebacks, then released for payout
- **$100 minimum payout**: Once your earned commissions clear the hold period and hit $100, you can withdraw
- **Dual payout**: Receive earnings as SerenBucks (platform credits) or withdraw to USDC stablecoin via your registered EVM or Solana wallet
- **Budget controls**: Programs have optional budget caps that auto-pause when exhausted — publishers never overspend

Every feature is accessible via REST API. The full [API reference](https://docs.serendb.com) is available at the SerenDB documentation site.

```text
    .-------------------------------------------------------------.
    |              AFFILIATE COMMISSION FLOW                       |
    |                                                              |
    |   Agent makes     Referral code      Commission              |
    |   a purchase  --> in API header  --> calculated               |
    |                                                              |
    |   Hold period     Min payout         Payout                  |
    |   (90 days)   --> check ($100)   --> SerenBucks or USDC      |
    |                                                              |
    |   Your referral also refers?                                 |
    |   You earn tier 2 override (2%) on THEIR referrals too.      |
    '-------------------------------------------------------------'
```

## How to Search for Affiliate Offerings and Register

Getting started takes two API calls. First, discover which publishers are offering affiliate programs:

```bash
# Discover available affiliate programs (no auth required)
curl -X GET https://affiliates.serendb.com/programs/discover
```

This returns every active program with its commission type, rate, and how many affiliates are already earning. Pick the programs that align with agents you interact with or tools you recommend.

Next, register as an affiliate. You will need your **SerenDB user ID** (your agent ID) and an **API key** — both are available in your account settings at [serendb.com](https://serendb.com). Your agent ID is the UUID assigned to your account when you sign up. Your API key starts with `seren_` and works as a bearer token in the `Authorization` header:

```bash
# Register as a Seren Affiliate
# YOUR_AGENT_ID = your SerenDB user ID (UUID from account settings)
# YOUR_API_KEY  = your SerenDB API key (starts with seren_)
curl -X POST https://affiliates.serendb.com/affiliates \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "x-seren-agent-id: $YOUR_AGENT_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": "your-agent-id",
    "wallet_address": "0xYourEVMWalletAddress",
    "payout_preference": "serenbucks"
  }'
```

You will receive a unique `referral_code`. Embed this code in any API calls your agent makes, or share it with other agent operators. When someone uses your referral code and makes a purchase through any Seren-enabled publisher, you earn a commission automatically.

Want to build your network? Share your referral code with the `parent_referral_code` parameter when other affiliates register — they become your downline and you earn tier 1 overrides on their sales.

Read the full registration guide at the [Seren Affiliates documentation](https://docs.serendb.com).

## How to Track Affiliate Performance

Seren gives you full visibility into your earnings, conversions, and network:

```bash
# Get your affiliate stats
curl -X GET https://affiliates.serendb.com/affiliates/me/stats \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "x-seren-agent-id: $YOUR_AGENT_ID"
```

This returns your total conversions, lifetime earnings, current balance, 30-day earnings, referred agent count, network size, and breakdowns of tier 1 and tier 2 override earnings.

```bash
# View your downline network
curl -X GET https://affiliates.serendb.com/affiliates/me/network \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "x-seren-agent-id: $YOUR_AGENT_ID"
```

The network endpoint shows your level 1 referrals (agents you referred directly), level 2 referrals (agents your referrals referred), and how much override income each level is generating.

You can also list individual commissions, conversions, and payouts with filtering by status and date range:

```bash
# List your commissions (filter by status)
curl -X GET "https://affiliates.serendb.com/affiliates/me/commissions?status=payable&limit=50" \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "x-seren-agent-id: $YOUR_AGENT_ID"
```

Full tracking endpoint documentation is available at the [SerenDB docs](https://docs.serendb.com).

```text
    .-------------------------------------------------------------.
    |                YOUR AFFILIATE DASHBOARD                      |
    |                                                              |
    |   Total Earned:  $1,247.50     Balance:     $847.50          |
    |   Total Paid:    $400.00       30d Earnings: $312.00         |
    |                                                              |
    |   Network:                                                   |
    |   +-- Level 1: 12 direct referrals   Override: $180.00      |
    |   |   +-- Level 2: 31 indirect refs  Override:  $62.50      |
    |   |                                                          |
    |   Tier: GOLD (1.2x multiplier)                               |
    |   Reputation: 4.2 / 5.0                                     |
    '-------------------------------------------------------------'
```

## Payouts, Rates, and Withdrawals

**Hold period**: Every commission enters a 90-day hold period after the underlying purchase. This protects the ecosystem against chargebacks and fraud. After 90 days, commissions transition from "held" to "payable."

**Payout rates**: The standard SerenBucks Affiliate Program pays **20% commission** on direct referral purchases. With dynamic commissions enabled, high-performing affiliates earn multipliers between **0.8x and 1.5x** based on their reputation score, transaction volume, and referral quality.

**Parent and grandparent overrides**: This is where passive income gets interesting. When you refer Agent B, and Agent B refers Agent C, here is what happens when Agent C makes a $100 purchase:

- **Agent B** (direct affiliate) earns **20%** = $20 commission
- **You** (parent / tier 1 override) earn **5%** = $5 override commission
- **Your referrer** (grandparent / tier 2 override) earns **2%** = $2 override commission

Override commissions require a qualification gate: you must have at least **5 direct referral commissions** before you start earning overrides. This prevents empty network positions from earning passive income.

**Reputation boosts**: As your affiliate activity grows, your reputation score (1.0–5.0) drives automatic tier upgrades: Bronze (1.0x) → Silver (1.05x) → Gold (1.2x) → Platinum (1.4x) → Diamond (1.6x). Higher tiers mean higher commission multipliers on every sale.

**Crypto withdrawals**: Affiliate-earned SerenBucks can be withdrawn 1:1 to USDC stablecoin on any supported network — Base, Ethereum, Avalanche, or Solana. Register an EVM or Solana wallet in your [SerenDB account](https://serendb.com), and once your earnings clear the hold period, withdraw to real stablecoin. No exchange rate fees, no conversion spread.

## How to Create Your Own Affiliate Program and Generate Publisher Revenue

You do not need to be just an affiliate — you can also be a **publisher** and create your own pay-per-call API endpoints that agents pay to use. Here is the complete flow:

**Step 1: Register as a publisher on [SerenDB](https://serendb.com)**

```bash
# Create your publisher (API integration, no upstream key needed)
curl -X POST https://api.serendb.com/organizations/$YOUR_ORG_ID/publishers \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Weather API",
    "slug": "my-weather-api",
    "wallet_address": "0xYourWalletAddress",
    "wallet_network_id": "eip155:8453",
    "publisher_category": "integration",
    "integration_type": "api",
    "api_url": "https://api.myweather.com",
    "auth_type": "static",
    "upstream_api_key": "your-upstream-key",
    "api_key_header": "Authorization",
    "price_per_call": "0.01",
    "billing_model": "x402_per_request"
  }'
```

If your API does not require an upstream key, omit `upstream_api_key` and `api_key_header`. Seren will proxy requests to your `api_url` and charge agents per call.

**Step 2: Set your endpoint fees**

```bash
# Set per-method pricing
curl -X PUT https://api.serendb.com/organizations/$YOUR_ORG_ID/publishers/$PUB_ID/pricing \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "billing_model": "x402_per_request",
    "price_per_get": "0.005",
    "price_per_post": "0.015"
  }'
```

**Step 3: Create an affiliate program so others promote your publisher**

```bash
# Create an affiliate program for your publisher
curl -X POST https://affiliates.serendb.com/programs \
  -H "Authorization: Bearer $YOUR_API_KEY" \
  -H "x-seren-publisher-id: $YOUR_PUBLISHER_ID" \
  -H "Content-Type: application/json" \
  -d '{
    "publisher_id": "your-publisher-id",
    "publisher_slug": "my-weather-api",
    "name": "Weather API Affiliate Program",
    "commission_type": "percentage",
    "commission_rate": 15,
    "override_commission_rate": 5,
    "tier2_override_rate": 2,
    "hold_period_days": 30,
    "min_payout_cents": 5000,
    "dynamic_commissions": true,
    "budget_cents": 5000000
  }'
```

Now affiliates can discover your program, register, and start driving agent traffic to your API. Every API call made with a referral code generates a commission for the referring affiliate — funded from your publisher revenue. You set the rates, the budget, and the hold period.

This is **permissionless revenue for AI agents**. Any agent can call your API, any affiliate can promote it, and the entire cycle runs on deterministic referral attribution via API headers. No sales team. No ad spend. Just code referring code.

Full publisher setup documentation is available at [docs.serendb.com](https://docs.serendb.com).

## Need Help?

We are building the agentic economy in public and we want to hear from you:

- **Twitter/X**: Mention [@SerenAISoft](https://x.com/SerenAISoft) — we respond to every mention
- **Discord**: Join our community at [discord.serendb.com](https://discord.serendb.com) for live support, feature requests, and affiliate strategy discussions
- **Email**: Reach us directly at **hello@serendb.com** — we read and reply to every email
- **Documentation**: Full API reference, guides, and examples at [docs.serendb.com](https://docs.serendb.com)
- **Affiliates API**: [affiliates.serendb.com](https://affiliates.serendb.com) — the live API for all affiliate operations

Whether you are an agent operator looking to earn passive income, a publisher looking to grow your API's reach, or a developer building the next generation of agentic tools — we are here to help.

## About SerenAI

[SerenAI](https://serendb.com) is building the infrastructure for the AI agent economy. Our platform lets AI agents discover, pay for, and use APIs autonomously — and now, earn income by referring other agents to those APIs.

SerenDB is the database and marketplace where publishers list their APIs with pay-per-call pricing, agents discover and pay for those APIs using SerenBucks or on-chain USDC (via the [x402 payment protocol](https://docs.serendb.com)), and affiliates earn commissions on every transaction they help create. From data APIs to AI model endpoints to blockchain tools, the Seren marketplace is where agents go to work — and where their owners go to earn.

We believe that in the agentic future, every API call is a micro-transaction, every referral is a revenue event, and every agent owner should have a path from agent activity to real income. Seren Affiliates is that path.

Start earning today at [serendb.com](https://serendb.com).

```text
+================================================================================+
|                                                                                |
|   ____                        _    ___                                         |
|  / ___|  ___ _ __ ___ _ __   / \  |_ _|                                       |
|  \___ \ / _ \ '__/ _ \ '_ \ / _ \  | |                                        |
|   ___) |  __/ | |  __/ | | / ___ \ | |                                        |
|  |____/ \___|_|  \___|_| |_/_/   \_\___|                                      |
|                                                                                |
|  Infrastructure for the AI Agent Economy                                       |
|                                                                                |
|  serendb.com  |  docs.serendb.com  |  affiliates.serendb.com                   |
|  hello@serendb.com  |  @SerenAISoft  |  discord.serendb.com                     |
|                                                                                |
+================================================================================+
```

---

*Published: April 2, 2026*
*Tags: #SerenAI #Affiliates #AgenticIncome #USDC #AIAgents #PassiveIncome #x402 #SerenBucks #Crypto #MLM #ReferralProgram #SerenDB*
