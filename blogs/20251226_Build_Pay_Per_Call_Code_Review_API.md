# Build a Pay-Per-Call Code Review API in Rust with x402 Micropayments

*Turn your AI wrapper into a revenue stream. This tutorial walks through building a Code Review API in Rust on Cloudflare Workers and monetizing it via SerenAI's x402 gateway.*

---

## The Opportunity

CodeRabbit, Cursor, and other code review tools have proven the market—developers pay for AI-powered code analysis. But most charge monthly subscriptions, leaving money on the table from:

- **Occasional users** who won't commit to $20/month
- **AI agents** that need programmatic access without human signup
- **CI/CD pipelines** that want per-review pricing

What if you could build a Code Review API and charge $0.01-0.05 per review, settled instantly in USDC? No subscriptions. No invoicing. Just pay-per-call.

This tutorial shows you how—in Rust.

## What We're Building

A Code Review API that:

1. Accepts code snippets or file contents
2. Analyzes for security, performance, and best practices using Claude
3. Returns structured findings with severity and fix suggestions
4. Charges per request via x402 micropayments

**Stack:**
- Cloudflare Workers (Rust compiled to WASM)
- Claude API for analysis
- SerenAI x402 Gateway for payments

## Step 1: Create the Rust Worker Project

Initialize a new Rust Worker:

```bash
cargo generate cloudflare/workers-rs
# Project name: code-review-api
cd code-review-api
```

Update `Cargo.toml`:

```toml
[package]
name = "code-review-api"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
worker = "0.4"
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
regex = "1.10"

[profile.release]
opt-level = "s"
lto = true
```

## Step 2: Write the Rust Code

Replace `src/lib.rs`:

```rust
use serde::{Deserialize, Serialize};
use worker::*;

#[derive(Deserialize)]
struct ReviewRequest {
    code: String,
    language: Option<String>,
    focus: Option<Vec<String>>,
}

#[derive(Serialize, Deserialize)]
struct Finding {
    severity: String,
    category: String,
    line: Option<u32>,
    message: String,
    suggestion: Option<String>,
}

#[derive(Serialize, Deserialize)]
struct ReviewResponse {
    findings: Vec<Finding>,
    summary: String,
    score: u32,
}

#[derive(Serialize)]
struct ClaudeMessage {
    role: String,
    content: String,
}

#[derive(Serialize)]
struct ClaudeRequest {
    model: String,
    max_tokens: u32,
    system: String,
    messages: Vec<ClaudeMessage>,
}

#[derive(Deserialize)]
struct ClaudeContentBlock {
    text: Option<String>,
}

#[derive(Deserialize)]
struct ClaudeResponse {
    content: Vec<ClaudeContentBlock>,
}

const SYSTEM_PROMPT: &str = r#"You are a senior code reviewer. Analyze the provided code for:
- Security vulnerabilities (injection, XSS, secrets exposure, auth issues)
- Performance problems (N+1 queries, memory leaks, inefficient algorithms)
- Best practices violations (error handling, naming, code organization)

Return a JSON object with:
{
  "findings": [
    {
      "severity": "critical|high|medium|low|info",
      "category": "security|performance|best-practices",
      "line": <line number if applicable>,
      "message": "<what's wrong>",
      "suggestion": "<how to fix>"
    }
  ],
  "summary": "<1-2 sentence overview>",
  "score": <0-100 quality score>
}

Be specific. Reference line numbers when possible. Prioritize actionable feedback."#;

fn json_response(body: &str, status: u16) -> Result<Response> {
    let mut headers = Headers::new();
    headers.set("Content-Type", "application/json")?;
    Ok(Response::ok(body)?.with_status(status).with_headers(headers))
}

fn error_response(message: &str, status: u16) -> Result<Response> {
    let body = serde_json::json!({ "error": message }).to_string();
    json_response(&body, status)
}

async fn handle_review(mut req: Request, env: Env) -> Result<Response> {
    // Parse request body
    let body: ReviewRequest = match req.json().await {
        Ok(b) => b,
        Err(_) => return error_response("Invalid JSON body", 400),
    };

    if body.code.trim().is_empty() {
        return error_response("Code is required", 400);
    }

    // Build the prompt
    let focus_areas = body.focus
        .map(|f| f.join(", "))
        .unwrap_or_else(|| "security, performance, best-practices".to_string());
    let language = body.language.unwrap_or_else(|| "auto-detect".to_string());

    let user_prompt = format!(
        "Language: {}\nFocus areas: {}\n\nCode to review:\n```\n{}\n```",
        language, focus_areas, body.code
    );

    // Get API key from environment
    let api_key = match env.secret("ANTHROPIC_API_KEY") {
        Ok(key) => key.to_string(),
        Err(_) => return error_response("API key not configured", 500),
    };

    // Build Claude request
    let claude_request = ClaudeRequest {
        model: "claude-sonnet-4-20250514".to_string(),
        max_tokens: 2048,
        system: SYSTEM_PROMPT.to_string(),
        messages: vec![ClaudeMessage {
            role: "user".to_string(),
            content: user_prompt,
        }],
    };

    // Call Claude API
    let mut headers = Headers::new();
    headers.set("Content-Type", "application/json")?;
    headers.set("x-api-key", &api_key)?;
    headers.set("anthropic-version", "2023-06-01")?;

    let mut init = RequestInit::new();
    init.with_method(Method::Post);
    init.with_headers(headers);
    init.with_body(Some(serde_json::to_string(&claude_request)?.into()));

    let request = Request::new_with_init(
        "https://api.anthropic.com/v1/messages",
        &init,
    )?;

    let mut response = Fetch::Request(request).send().await?;

    if response.status_code() != 200 {
        let error_text = response.text().await.unwrap_or_default();
        console_log!("Claude API error: {}", error_text);
        return error_response("Analysis failed", 502);
    }

    let claude_response: ClaudeResponse = response.json().await?;
    let content = claude_response
        .content
        .first()
        .and_then(|c| c.text.clone())
        .unwrap_or_default();

    // Extract JSON from response
    let json_regex = regex::Regex::new(r"\{[\s\S]*\}").unwrap();
    let json_match = match json_regex.find(&content) {
        Some(m) => m.as_str(),
        None => return error_response("Failed to parse analysis", 500),
    };

    // Parse and return the review
    let review: ReviewResponse = match serde_json::from_str(json_match) {
        Ok(r) => r,
        Err(_) => return error_response("Failed to parse analysis JSON", 500),
    };

    json_response(&serde_json::to_string(&review)?, 200)
}

#[event(fetch)]
async fn fetch(req: Request, env: Env, _ctx: Context) -> Result<Response> {
    // Route: POST /review
    if req.method() != Method::Post {
        return error_response("Method not allowed", 405);
    }

    let path = req.path();
    if path != "/review" {
        return error_response("Not found", 404);
    }

    handle_review(req, env).await
}
```

## Step 3: Configure and Deploy

Add your Anthropic API key:

```bash
npx wrangler secret put ANTHROPIC_API_KEY
```

Update `wrangler.toml`:

```toml
name = "code-review-api"
main = "build/worker/shim.mjs"
compatibility_date = "2024-01-01"

[build]
command = "cargo install -q worker-build && worker-build --release"
```

Deploy:

```bash
npx wrangler deploy
```

You'll get a URL like `https://code-review-api.<your-subdomain>.workers.dev`.

## Step 4: Test Your API

```bash
curl -X POST https://code-review-api.<your-subdomain>.workers.dev/review \
  -H "Content-Type: application/json" \
  -d '{
    "code": "fn login(user: &str, pass: &str) -> Result<User, Error> {\n  let query = format!(\"SELECT * FROM users WHERE user={}\", user);\n  db.query(&query)\n}",
    "language": "rust",
    "focus": ["security"]
  }'
```

Expected response:

```json
{
  "findings": [
    {
      "severity": "critical",
      "category": "security",
      "line": 2,
      "message": "SQL injection vulnerability. User input is directly interpolated into query string using format!().",
      "suggestion": "Use parameterized queries: db.query(\"SELECT * FROM users WHERE user = $1\", &[user])"
    }
  ],
  "summary": "Critical SQL injection vulnerability detected. Use parameterized queries instead of string interpolation.",
  "score": 15
}
```

## Step 5: Register as a SerenAI Publisher

Now monetize your API. Sign up at [console.serendb.com/signup](https://console.serendb.com/signup) with invite code `serenai2025`.

Or register via API:

```bash
curl -X POST https://x402.serendb.com/api/publishers/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Code Review API",
    "email": "you@example.com",
    "walletAddress": "0xYourWalletAddress",
    "connectionString": "postgresql://user:pass@your-serendb-instance.serendb.com:5432/dbname",
    "publisherType": "api",
    "resourceName": "Rust Code Review",
    "resourceDescription": "AI-powered code review for security, performance, and best practices. Built in Rust.",
    "upstreamApiUrl": "https://code-review-api.your-subdomain.workers.dev",
    "categories": ["code-review", "security", "ai", "developer-tools", "rust"],
    "billingModel": "prepaid",
    "usageExample": {
      "method": "POST",
      "path": "/review",
      "description": "Analyze code for security vulnerabilities and best practices",
      "params": {
        "code": { "required": true, "type": "string", "description": "Code to review" },
        "language": { "required": false, "type": "string", "description": "Programming language" },
        "focus": { "required": false, "type": "array", "description": "Areas: security, performance, best-practices" }
      }
    }
  }'
```

Save the returned `apiKey`—you'll need it for publisher management.

## Step 6: Pricing Strategy

With the **prepaid** billing model, you charge based on actual usage. Your Worker calculates costs dynamically based on code size and returns the amount in the response.

Your costs per review:

- **Claude API**: ~$0.003 per 1K input tokens, ~$0.015 per 1K output tokens
- **Cloudflare Workers**: Negligible (free tier covers 100K requests/day)

**Usage-based pricing formula:**

```rust
fn calculate_price(code_lines: usize) -> f64 {
    let base_price = 0.005;  // $0.005 minimum
    let per_line = 0.00005;  // $0.00005 per line
    base_price + (code_lines as f64 * per_line)
}
// 100 lines = $0.01, 1000 lines = $0.055, 10000 lines = $0.505
```

This ensures you maintain margins regardless of code size. Include the calculated price in your response so the gateway can deduct from the caller's prepaid balance.

## Step 7: AI Agents Can Now Use Your API

Once registered, AI agents discover your API via the catalog:

```bash
curl https://x402.serendb.com/api/catalog
```

And call it with payment:

```bash
curl -X POST https://x402.serendb.com/api/proxy \
  -H "Content-Type: application/json" \
  -d '{
    "publisherId": "<your-publisher-id>",
    "agentWallet": "0xAgentWallet...",
    "request": {
      "method": "POST",
      "path": "/review",
      "body": {
        "code": "fn main() { unsafe { std::ptr::null::<i32>().read() } }",
        "language": "rust",
        "focus": ["security"]
      }
    }
  }'
```

The gateway handles x402 payment flow. You receive USDC directly to your wallet.

## Why Rust?

Building on Cloudflare Workers with Rust gives you:

- **Performance**: WASM execution with near-native speed
- **Memory safety**: No buffer overflows or null pointer bugs in your API
- **Small binary**: Aggressive LTO optimization for fast cold starts
- **Type safety**: Catch errors at compile time, not runtime

The same Worker handles thousands of concurrent requests with minimal memory footprint.

## GitHub Integration (Like CodeRabbit)

Most developers want code reviews where they already work—GitHub PRs. Here's how to build a GitHub App that automatically reviews pull requests.

### Step 1: Create a GitHub App

1. Go to **Settings → Developer settings → GitHub Apps → New GitHub App**
2. Configure:
   - **Name**: Your Code Review Bot
   - **Webhook URL**: `https://code-review-api.<subdomain>.workers.dev/webhook`
   - **Permissions**:
     - Pull requests: Read & Write
     - Contents: Read
   - **Subscribe to events**: Pull request, Pull request review

3. Generate and download the private key

### Step 2: Add Webhook Handler

Extend your Worker to handle GitHub webhooks:

```rust
use hmac::{Hmac, Mac};
use sha2::Sha256;

#[derive(Deserialize)]
struct PullRequestEvent {
    action: String,
    pull_request: PullRequest,
    repository: Repository,
}

#[derive(Deserialize)]
struct PullRequest {
    number: u32,
    head: GitRef,
    base: GitRef,
}

#[derive(Deserialize)]
struct GitRef {
    sha: String,
    #[serde(rename = "ref")]
    ref_name: String,
}

#[derive(Deserialize)]
struct Repository {
    full_name: String,
}

async fn handle_webhook(mut req: Request, env: Env) -> Result<Response> {
    // Verify GitHub signature
    let signature = req.headers().get("X-Hub-Signature-256")?.unwrap_or_default();
    let body = req.text().await?;

    let webhook_secret = env.secret("GITHUB_WEBHOOK_SECRET")?.to_string();
    if !verify_signature(&body, &signature, &webhook_secret) {
        return error_response("Invalid signature", 401);
    }

    let event: PullRequestEvent = serde_json::from_str(&body)?;

    // Only review on PR open or synchronize (new commits)
    if event.action != "opened" && event.action != "synchronize" {
        return json_response(r#"{"status": "ignored"}"#, 200);
    }

    // Fetch the PR diff
    let diff = fetch_pr_diff(&event, &env).await?;

    // Review each changed file
    let mut all_findings = Vec::new();
    for file in diff.files {
        if let Some(patch) = file.patch {
            let review = analyze_code(&patch, &file.filename, &env).await?;
            all_findings.extend(review.findings);
        }
    }

    // Post review comments to GitHub
    post_review_comments(&event, &all_findings, &env).await?;

    json_response(r#"{"status": "reviewed"}"#, 200)
}

async fn post_review_comments(
    event: &PullRequestEvent,
    findings: &[Finding],
    env: &Env,
) -> Result<()> {
    let token = get_installation_token(env).await?;
    let repo = &event.repository.full_name;
    let pr_number = event.pull_request.number;

    // Create a review with inline comments
    let review_body = serde_json::json!({
        "body": format!("## Code Review\n\nFound {} issues.", findings.len()),
        "event": "COMMENT",
        "comments": findings.iter().map(|f| {
            serde_json::json!({
                "path": f.file.clone().unwrap_or_default(),
                "line": f.line.unwrap_or(1),
                "body": format!(
                    "**{}** ({})\n\n{}\n\n💡 **Suggestion:** {}",
                    f.severity.to_uppercase(),
                    f.category,
                    f.message,
                    f.suggestion.clone().unwrap_or_default()
                )
            })
        }).collect::<Vec<_>>()
    });

    let url = format!(
        "https://api.github.com/repos/{}/pulls/{}/reviews",
        repo, pr_number
    );

    // POST to GitHub API
    let mut headers = Headers::new();
    headers.set("Authorization", &format!("Bearer {}", token))?;
    headers.set("Accept", "application/vnd.github+json")?;
    headers.set("User-Agent", "CodeReviewBot")?;

    let mut init = RequestInit::new();
    init.with_method(Method::Post);
    init.with_headers(headers);
    init.with_body(Some(review_body.to_string().into()));

    Fetch::Request(Request::new_with_init(&url, &init)?).send().await?;
    Ok(())
}
```

### Step 3: Update Your Router

```rust
#[event(fetch)]
async fn fetch(req: Request, env: Env, _ctx: Context) -> Result<Response> {
    let path = req.path();

    match (req.method(), path.as_str()) {
        (Method::Post, "/review") => handle_review(req, env).await,
        (Method::Post, "/webhook") => handle_webhook(req, env).await,
        _ => error_response("Not found", 404),
    }
}
```

### Step 4: Configure Secrets

```bash
npx wrangler secret put GITHUB_APP_ID
npx wrangler secret put GITHUB_PRIVATE_KEY
npx wrangler secret put GITHUB_WEBHOOK_SECRET
```

### How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                    GITHUB PR REVIEW FLOW                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Developer opens PR on GitHub                                │
│                    ↓                                            │
│  2. GitHub sends webhook to your Worker                         │
│                    ↓                                            │
│  3. Worker fetches PR diff via GitHub API                       │
│                    ↓                                            │
│  4. Worker sends code to Claude for review                      │
│                    ↓                                            │
│  5. Worker posts inline comments on the PR                      │
│                    ↓                                            │
│  6. Developer sees review comments in GitHub UI                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Monetization Options

For GitHub integration, consider these billing models:

| Model | Pricing | Best For |
|-------|---------|----------|
| **Per-PR** | $0.05-0.25 per PR reviewed | Small teams |
| **Per-repo/month** | $10-50/repo/month | Active projects |
| **Per-seat** | $5-15/user/month | Enterprise teams |

You can use SerenAI's prepaid credits model—teams deposit USDC and each PR review deducts from their balance automatically.

## Extending the API

Add more value to justify higher pricing:

**Multi-file analysis:**

```rust
#[derive(Deserialize)]
struct FileEntry {
    path: String,
    content: String,
}

#[derive(Deserialize)]
struct PrReviewRequest {
    files: Vec<FileEntry>,
    context: Option<String>,
}
```

**Specialized endpoints:**

- `POST /review/security` - Deep OWASP Top 10 audit
- `POST /review/performance` - Profiling suggestions
- `POST /review/rust` - Rust-specific idioms and safety checks

## Revenue Projections

At $0.01 per review:

| Daily Reviews | Monthly Revenue |
|---------------|-----------------|
| 100 | $30 |
| 1,000 | $300 |
| 10,000 | $3,000 |
| 100,000 | $30,000 |

CodeRabbit charges ~$15/user/month. Your pay-per-call model captures developers who want occasional reviews without commitment.

## Summary

You just built a monetized AI wrapper in Rust:

1. **Created** a Cloudflare Worker in Rust that calls Claude for code review
2. **Compiled** to WASM with aggressive optimization
3. **Deployed** to the edge with global distribution
4. **Registered** as a SerenAI publisher with x402 payments
5. **Priced** competitively with 70%+ margins

AI agents can now discover and pay for your service automatically. No API keys to manage. No subscriptions to bill. Just USDC flowing to your wallet per request.

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
*Tags: #Tutorial #SerenAI #x402 #CodeReview #Rust #CloudflareWorkers #WASM #Micropayments #USDC #Developer*
