# Show HN: Rust Cron Scheduler with x402 Payment Delegation (Cloudflare Workers)

*A Rust-based scheduler compiled to WASM that lets AI agents run recurring jobs with automatic micropayments—no interactive signing required.*

---

## The Problem: Scheduled Jobs Can't Sign Payments

We hit a wall. Our AI agents needed to call pay-per-query APIs (like Alpaca Finance for stock data) on a schedule. But the x402 protocol requires cryptographic signatures for each payment—EIP-3009 `transferWithAuthorization` signatures that need a wallet's private key at execution time.

```
Scheduler → API
           ↓
        402 Payment Required
           ↓
        ??? Who signs this at 3am?
```

Cron jobs run autonomously. There's no human to approve payments. There's no interactive wallet popup at 3am when your MSTR price monitor fires.

The naive solution—embedding the agent's private key in the scheduler—is a security nightmare. One compromised Worker means drained wallets.

**We needed the scheduler to pay without holding keys.**

## Background: The x402 Protocol

x402 is an open HTTP payment protocol (championed by Coinbase) that uses the HTTP 402 status code. When an API requires payment, it returns HTTP 402 with payment requirements:

```json
{
  "x402Version": 1,
  "accepts": [{
    "scheme": "exact",
    "network": "base",
    "maxAmountRequired": "300",
    "payTo": "0x..."
  }]
}
```

The client must sign an EIP-3009 `transferWithAuthorization`—a gasless USDC transfer authorization—and retry with an `X-PAYMENT` header containing the signature. The server verifies, settles on-chain, and returns data.

This works great for interactive clients. For autonomous schedulers? Not so much.

## The Solution: X-Payment-Delegation

We built payment delegation into our x402 gateway. When an automated service sends `X-Payment-Delegation: true`, the gateway handles the entire payment flow on behalf of the caller:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PAYMENT DELEGATION FLOW                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Agent deposits USDC → Prepaid Credits (one-time)            │
│                                                                  │
│  2. Scheduler → Gateway (X-Payment-Delegation: true)            │
│                    ↓                                             │
│  3. Gateway → Upstream API                                       │
│                    ↓                                             │
│              402 Payment Required                                │
│                    ↓                                             │
│  4. Gateway signs EIP-3009 from gateway wallet                  │
│     Gateway deducts from agent's prepaid credits                │
│                    ↓                                             │
│  5. Gateway → Upstream (with X-PAYMENT header)                  │
│                    ↓                                             │
│  6. Success! Response returned to Scheduler                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

The gateway acts as a payment facilitator. It holds USDC in its own wallet, signs payments from that wallet, and reconciles against each agent's prepaid credit balance. The scheduler never touches private keys. The agent never shares their keys with anyone.

## Implementation

### Gateway Side (TypeScript/Express)

The gateway detects the delegation header and handles upstream 402 responses:

```typescript
// Detect delegation mode
const isDelegatedPayment = req.header('X-Payment-Delegation') === 'true';

// Verify agent has prepaid credits before proceeding
if (isDelegatedPayment) {
  const hasBalance = await hasSufficientBalance(agentWallet, minimumBalance);
  if (!hasBalance) {
    return res.status(402).json({
      error: 'Insufficient credit balance for delegated payment',
      depositEndpoint: '/api/credits/confirm-deposit',
    });
  }
}

// When upstream returns 402...
if (isDelegatedPayment && upstreamRequirements.length > 0) {
  const requirement = upstreamRequirements[0];

  // Sign EIP-3009 transferWithAuthorization from gateway wallet
  const signedPayment = await createGatewaySignedPayment(requirement);

  // Retry upstream with signed payment
  const response = await forwardToUpstream(upstream, request, {
    paymentHeader: signedPayment.paymentHeader,
  });

  // Deduct from agent's prepaid credits
  await recordPrepaidUsage(agentWallet, publisherId, signedPayment.amountDecimal);

  return res.json({ ...response.body, _x402: { delegatedPayment: true } });
}
```

The `createGatewaySignedPayment` function generates EIP-712 typed data signatures for USDC's `transferWithAuthorization`. This is the same signature format used by Circle's USDC contract for gasless transfers:

```typescript
const signature = await wallet.signTypedData({
  account: facilitatorAccount,
  domain: buildEip712Domain(), // USDC contract on Base
  types: TRANSFER_WITH_AUTH_TYPES,
  primaryType: 'TransferWithAuthorization',
  message: {
    from: gatewayAddress,
    to: requirement.payTo,
    value: BigInt(requirement.maxAmountRequired),
    validAfter: BigInt(now - 60),   // 1 min buffer for clock skew
    validBefore: BigInt(now + 300), // 5 min validity window
    nonce: toHex(crypto.getRandomValues(new Uint8Array(32)), { size: 32 }),
  },
});
```

### Scheduler Side (Rust/Cloudflare Workers)

SerenCron is our Rust-based cron scheduler running on Cloudflare Workers. The entire payment integration is a single header:

```rust
let headers = Headers::new();
headers.set("Content-Type", "application/json")?;
headers.set("X-Payment-Delegation", "true")?;

let request = serde_json::json!({
    "publisherId": publisher_id,
    "agentWallet": job.agent_wallet,
    "request": {
        "method": "GET",
        "path": "/v2/stocks/MSTR/quotes/latest",
        "query": {}
    }
});
```

The scheduler extracts cost metadata from gateway responses for job execution tracking:

```rust
fn extract_x402_cost(body: &str) -> Option<f64> {
    let json: serde_json::Value = serde_json::from_str(body).ok()?;
    // Delegated payments return decimal amounts in _x402.amountPaid
    json.get("_x402")?.get("amountPaid")?.as_str()?.parse().ok()
}
```

This cost gets stored in the execution result, giving agents visibility into exactly what each scheduled job costs.

## Live Test: MSTR Quote via Alpaca

We tested with MicroStrategy stock (MSTR) via the Alpaca Finance publisher:

```bash
# Via x402 gateway with payment delegation
$ curl -X POST https://x402.serendb.com/api/proxy \
    -H "Content-Type: application/json" \
    -H "X-Payment-Delegation: true" \
    -d '{
      "publisherId": "36454f6e-2b72-4341-8414-53a6da7da978",
      "agentWallet": "0xYourWallet...",
      "request": { "method": "GET", "path": "/v2/stocks/MSTR/quotes/latest" }
    }'

{
  "quote": {
    "ap": 160.00,
    "bp": 155.00,
    "t": "2025-12-26T17:21:10Z"
  },
  "symbol": "MSTR",
  "_x402": {
    "paymentSettled": true,
    "delegatedPayment": true,
    "gatewayPayer": "0x...",
    "amountPaid": "0.0003",
    "upstreamPayTo": "0x..."
  }
}
```

$0.0003 USDC paid automatically. No wallet popup. No human intervention. Settlement confirmed on Base mainnet in the same request cycle.

## Security Model

Payment delegation is designed with defense in depth:

- **Gateway wallet holds USDC**: Makes payments on behalf of agents, not from agent wallets
- **Agent prepaid balance checked first**: Gateway verifies sufficient credits before making any payment—no overdrafts possible
- **Per-job budget limits**: Jobs can set `max_payment_per_call` to cap exposure per execution
- **All payments logged**: Every delegated payment records agent wallet, publisher, amount, timestamp, and upstream recipient
- **Gateway is reimbursed**: Credits deducted from agent balance equal payments made by gateway

The agent never shares private keys. The gateway never accesses agent funds directly—only the prepaid credit balance the agent explicitly deposited via on-chain USDC transfer.

## Use Cases

| Service | Description |
|---------|-------------|
| **SerenCron** | Scheduled jobs calling pay-per-query APIs on recurring schedules |
| **AI Agents** | Autonomous agents (Claude, GPT) making x402 API calls via MCP (Model Context Protocol) |
| **CI/CD Pipelines** | Build processes querying paid data sources for tests or deployments |
| **Backend Services** | Server-side integrations with x402-enabled publishers |

Any service that needs to make x402 payments without interactive signing can use delegation.

## Build Your Own Publisher

SerenCron is itself a publisher on the x402 gateway. You can register your own API and start accepting USDC micropayments from AI agents.

**1. Register via console:**
Sign up at [console.serendb.com](https://console.serendb.com/signup) and create a publisher with your upstream API URL.

**2. Configure pricing:**

```json
{
  "price_per_call": "0.0001",
  "price_per_get": "0.0001",
  "price_per_post": "0.0005",
  "wallet_address": "0xYourWallet..."
}
```

**3. Receive payments:**
When agents call your API through the gateway, USDC flows directly to your wallet. The gateway handles 402 responses, payment verification, and settlement—you just serve your API.

Publishers keep 100% of GET request payments. POST requests with publisher-set pricing include a 5% gateway fee. No subscription tiers, no usage caps, no invoicing.

**Discovery for LLMs:**

```bash
# Discover existing publishers
curl https://x402.serendb.com/api/catalog

# Learn publisher registration schema (returns validation errors showing all fields)
curl -X POST https://x402.serendb.com/api/publishers/register \
  -H "Content-Type: application/json" -d '{}'
```

AI agents can discover publishers via the catalog, or learn the registration schema by calling the register endpoint—Zod validation errors document all required and optional fields.

## Acknowledgments

Special thanks to **Cloudflare** for the Workers credits that made SerenCron development possible. Running a Rust-based scheduler compiled to WASM on their edge network—with fast cold starts and global distribution—has been excellent for reliable cron job execution.

The x402 protocol from Coinbase provided the foundation. EIP-3009's `transferWithAuthorization` made gasless USDC payments practical.

---

## About SerenAI

SerenAI is building payment infrastructure for the AI agent economy. Our x402 Gateway enables AI agents to autonomously pay for data and services using USDC micropayments on Base—no subscriptions, no API keys, no human intervention.

**Our Stack:** TypeScript, Rust, PostgreSQL, Cloudflare Workers, Base (Ethereum L2), Coinbase x402 Protocol

**Data Publishers:** Alpaca, CoinGecko, Firecrawl, Perplexity, OpenAI, Moonshot AI, CoinMarketCap, and 20+ more

---

> **Ready to build?** Sign up at **[console.serendb.com/signup](https://console.serendb.com/signup)** with invite code **`serenai2025`**

*Questions? Email hello@serendb.com or join our [Discord](https://discord.gg/jseg7q4KS7).*

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
║     │  TypeScript  •  Rust  •  Cloudflare Workers  •  Base  •  x402   │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║     PUBLISHERS:                                                              ║
║     📈 Alpaca  •  🦎 CoinGecko  •  🔥 Firecrawl  •  🔍 Perplexity          ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: December 26, 2025*
*Tags: #ShowHN #SerenAI #x402 #CronJobs #PaymentDelegation #Rust #CloudflareWorkers #AIAgents #USDC #Base #EIP3009*
