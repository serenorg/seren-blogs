# Show HN: How We Deployed MCP-to-MCP Communications in Seren's MCP Server

*Publishers can now connect their MCP servers directly to [Seren](https://serendb.com)—no REST APIs, no webhook plumbing, just native MCP protocol with pay-per-call built in.*

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║           THE OLD WAY                      THE NEW WAY                       ║
║                                                                              ║
║      ┌─────────┐    REST    ┌─────────┐       ┌─────────┐    MCP    ┌─────────┐
║      │  AGENT  │ ─────────▶ │   API   │       │  AGENT  │ ─────────▶│   MCP   │
║      │   MCP   │            │ GATEWAY │       │   MCP   │           │ SERVER  │
║      └─────────┘            └─────────┘       └─────────┘           └─────────┘
║           │                      │                 │                     │
║           │   HTTP + JSON        │                 │   Native Protocol   │
║           │   API Keys           │                 │   Type Safety       │
║           │   Rate Limiting      │                 │   Streaming         │
║           ▼                      ▼                 ▼                     ▼
║      "Authentication failed"   "429 Too Many"  ✓ Direct tool calls  ✓ Built-in billing
║                                                                              ║
║        ┌──────────────────────────────────────────────────────────────┐      ║
║        │   MCP-TO-MCP: WHEN YOUR AGENT SPEAKS YOUR PUBLISHER'S LANG  │      ║
║        └──────────────────────────────────────────────────────────────┘      ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

## Why We Built This

We launched Seren MCP Server to connect AI agents with data publishers. Our initial design routed everything through REST APIs—agents called our MCP server, we translated to HTTP, forwarded to upstream APIs, and passed responses back.

It worked. But when we onboarded new publishers, we kept hearing the same feedback:

> "We already have an MCP server. Can't you just talk to it directly?"

Fair point. These publishers had invested in building proper MCP servers with typed tool schemas, streaming support, and structured resources. Forcing them through a REST translation layer meant losing all that.

So we built MCP-to-MCP.

## The Architecture Change

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     SEREN MCP-TO-MCP ARCHITECTURE                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌──────────────┐         ┌──────────────────┐         ┌──────────────┐   │
│   │              │   MCP   │                  │   MCP   │              │   │
│   │    AGENT     │ ──────▶ │   SEREN GATEWAY  │ ──────▶ │  PUBLISHER   │   │
│   │  (Claude,    │         │                  │         │  MCP SERVER  │   │
│   │   Cursor)    │ ◀────── │  • Billing       │ ◀────── │              │   │
│   │              │         │  • Auth          │         │  (FastMCP,   │   │
│   └──────────────┘         │  • Discovery     │         │   mcp-sdk)   │   │
│                            └──────────────────┘         └──────────────┘   │
│                                     │                                       │
│                                     │ SerenBucks / x402                     │
│                                     ▼                                       │
│                            ┌──────────────────┐                             │
│                            │  PAYMENT LEDGER  │                             │
│                            │  Pay-per-call    │                             │
│                            └──────────────────┘                             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

The key insight: **Seren becomes an MCP-aware proxy, not a protocol translator.**

When an agent calls `call_mcp_tool(publisher="my-mcp-service", tool_name="analyze")`, Seren:

1. Authenticates the agent (OAuth or x402 signatures)
2. Opens an MCP connection to the publisher's endpoint
3. Forwards the `tools/call` request natively
4. Records the call for billing
5. Streams the response back to the agent

No REST translation. No JSON schema mismatches. The publisher's tool schemas flow through unchanged.

## What Publishers Get

**Native MCP protocol.** Your existing MCP server works as-is. If you built it with [FastMCP](https://github.com/jlowin/fastmcp) or the official mcp-sdk, you're ready.

**Instant monetization.** Set a price per tool call. Seren handles billing, settlement, and payment disputes. You get paid in USDC to your wallet.

**Discovery.** Agents find your tools via `list_mcp_tools`. Your tool descriptions become your marketing copy.

**No API key management.** Seren handles agent authentication. You just serve MCP requests.

## Registering Your MCP Server

Here's the complete flow to publish your MCP server on Seren:

```bash
# 1. Register as an MCP publisher
curl -X POST "https://api.serendb.com/agent/publishers" \
  -H "Authorization: Bearer $SEREN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Analysis Service",
    "slug": "my-analysis-service",
    "publisher_category": "integration",
    "integration_type": "mcp",
    "mcp_endpoint": "https://mcp.myservice.com/mcp",
    "wallet_address": "0x...",
    "wallet_network_id": "eip155:8453"
  }'

# 2. Configure pricing
curl -X PUT "https://api.serendb.com/agent/publishers/my-analysis-service/pricing" \
  -H "Authorization: Bearer $SEREN_API_KEY" \
  -d '{"price_per_call": "0.01", "prepaid_enabled": true}'
```

That's it. Agents can now discover and call your tools with `list_mcp_tools` and `call_mcp_tool`.

## How Agents Use MCP Publishers

From the agent's perspective, MCP publishers are first-class citizens:

```python
# Agent discovers your tools
tools = seren.list_mcp_tools(publisher="my-analysis-service")
# Returns: [{"name": "analyze_data", "description": "...", "inputSchema": {...}}]

# Agent calls your tool
result = seren.call_mcp_tool(
    publisher="my-analysis-service",
    tool_name="analyze_data",
    arguments={"data": [1, 2, 3, 4, 5]}
)
# Charges $0.01 to agent's SerenBucks balance
```

Resources work too. If your MCP server exposes resources, agents read them via `read_mcp_resource`.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           SUPPORTED MCP CAPABILITIES                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│    ┌────────────────┐  ┌────────────────┐  ┌────────────────┐              │
│    │     TOOLS      │  │   RESOURCES    │  │    PROMPTS     │              │
│    │                │  │                │  │                │              │
│    │  ✓ list        │  │  ✓ list        │  │  ✓ list        │              │
│    │  ✓ call        │  │  ✓ read        │  │  ✓ get         │              │
│    │  ✓ streaming   │  │  ✓ subscribe   │  │                │              │
│    │                │  │                │  │                │              │
│    └────────────────┘  └────────────────┘  └────────────────┘              │
│                                                                             │
│    Sampling: Not yet supported (v2 roadmap)                                 │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Technical Requirements

Your MCP server must:

- **Use HTTP/SSE transport** (Streamable HTTP). Stdio won't work—we need network connectivity.
- **Be publicly accessible.** Seren's gateway connects from our infrastructure.
- **Support standard MCP methods.** At minimum: `tools/list` and `tools/call`.

Most MCP frameworks handle this out of the box. Here's a minimal FastMCP example:

```python
from fastmcp import FastMCP

mcp = FastMCP("My Service")

@mcp.tool()
def analyze_data(data: list[float]) -> dict:
    """Analyze numerical data and return statistics."""
    return {"mean": sum(data) / len(data), "count": len(data)}

if __name__ == "__main__":
    mcp.run(transport="sse", port=8000)
```

Deploy this anywhere—Fly.io, Railway, your own VPS—and register the endpoint with Seren.

## What's Next

We're seeing publishers migrate from REST-based integrations to native MCP. The developer experience is better: no schema translation, proper streaming, typed tool signatures.

If you're building an MCP server and want to monetize it, check out our [MCP Publisher Guide](https://docs.serendb.com/guides/mcp-publishers.html). It covers authentication options, pricing models, and troubleshooting.

For agents, MCP publishers appear alongside our database and API publishers. Call `list_agent_publishers` to see everything available.

---

## Get Started

- **What is Seren?** [serendb.com](https://serendb.com)
- **MCP Publisher Guide:** [docs.serendb.com/guides/mcp-publishers](https://docs.serendb.com/guides/mcp-publishers.html)
- **Full API Docs:** [docs.serendb.com](https://docs.serendb.com)
- **Sign Up Free:** [console.serendb.com](https://console.serendb.com)
- **MCP Specification:** [modelcontextprotocol.io](https://modelcontextprotocol.io)

Questions? Drop them in comments or join our [Discord](https://discord.gg/jseg7q4KS7).

---

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
║              MCP-to-MCP: Native Protocol, Built-in Payments                  ║
║                                                                              ║
║     ┌──────────────────────────────────────────────────────────────────┐    ║
║     │  serendb.com  •  docs.serendb.com  •  console.serendb.com       │    ║
║     └──────────────────────────────────────────────────────────────────┘    ║
║                                                                              ║
║                  hello@serendb.com | discord.gg/jseg7q4KS7                   ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

---

*Published: January 19, 2026*
*Tags: #ShowHN #SerenAI #MCP #ModelContextProtocol #MCPServer #AIAgents #Micropayments #FastMCP*
