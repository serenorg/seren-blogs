# How AI Agents Can Now Manage Your CRM: Introducing Attio on Seren MCP

*Your AI agent can now source prospects from Crunchbase and Apollo, enrich them with company intelligence, and automatically populate your Attio CRM—all through [Seren MCP](https://serendb.com).*

---

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                                                                              ║
║                    THE AUTONOMOUS SALES PIPELINE                             ║
║                                                                              ║
║     ┌──────────────┐    ┌──────────────┐    ┌──────────────┐                ║
║     │  CRUNCHBASE  │    │    APOLLO    │    │   FIRECRAWL  │                ║
║     │  Companies   │    │   Contacts   │    │  Web Intel   │                ║
║     └──────┬───────┘    └──────┬───────┘    └──────┬───────┘                ║
║            │                   │                   │                         ║
║            └─────────┬─────────┴─────────┬─────────┘                         ║
║                      │                   │                                   ║
║                      ▼                   ▼                                   ║
║               ┌─────────────────────────────────┐                            ║
║               │         SEREN MCP SERVER        │                            ║
║               │     Orchestration + Billing     │                            ║
║               └─────────────────┬───────────────┘                            ║
║                                 │                                            ║
║                                 ▼                                            ║
║               ┌─────────────────────────────────┐                            ║
║               │           ATTIO CRM             │                            ║
║               │    Contacts • Companies • Deals │                            ║
║               └─────────────────────────────────┘                            ║
║                                                                              ║
║         YOUR AGENT BUILDS YOUR PIPELINE WHILE YOU SLEEP                      ║
║                                                                              ║
╚══════════════════════════════════════════════════════════════════════════════╝
```

## The CRM Problem AI Agents Can Finally Solve

Every sales team knows the pain: hours spent researching prospects, manually entering data into CRMs, and keeping contact information current. It's tedious work that distracts from what actually closes deals—building relationships.

What if your AI agent could handle all of this autonomously?

Today we're announcing [**Attio CRM as a publisher on Seren MCP**](https://serendb.com/bestsellers/attio). This means any AI agent—Claude, GPT, or your custom-built assistant—can now create contacts, manage companies, track deals, schedule tasks, and orchestrate your entire CRM workflow through natural language commands.

## What Attio + Seren MCP Enables

Attio is a modern CRM built for the AI era. Unlike legacy CRMs with rigid schemas, Attio adapts to how you actually work. Combined with Seren MCP, your agents gain full programmatic access to:

- **Contacts & Companies**: Create, update, search, and enrich records
- **Deals & Opportunities**: Track pipeline stages and revenue
- **Tasks & Meetings**: Schedule follow-ups and log activities
- **Notes & Comments**: Attach context to any record
- **Custom Objects**: Work with your unique data models

All accessible through simple tool calls, billed per operation through your [SerenBucks balance](https://serendb.com).

## Case Study: The Autonomous Prospecting Agent

Let's look at a real-world scenario that demonstrates the power of AI-driven CRM management.

### The Challenge

A B2B SaaS company needed to build a qualified prospect list for their sales acceleration product. Their Ideal Customer Profile (ICP) included:

- Series A+ funded startups
- Companies with 50-500 employees
- Industries: SaaS, FinTech, Healthcare Tech
- Decision makers: VP Sales, Head of BD, Revenue Operations

Traditionally, this would require a sales development rep spending 20+ hours per week manually researching companies, finding contacts, and entering data into the CRM.

### The AI Agent Solution

Using Seren MCP, they deployed an autonomous prospecting agent. Here's the full prompt you can copy and paste to try it yourself:

```
You are an autonomous B2B prospecting agent. Your task is to build a qualified
prospect list and populate my Attio CRM. Follow these steps:

## Step 1: Source Companies from Crunchbase
Use the Crunchbase publisher via Seren MCP to find companies matching:
- Funding stage: Series A or later
- Industry: SaaS, FinTech, or Healthcare Tech
- Founded: 2020 or later
- Employee count: 50-500
- Location: United States

For each company found, extract: company name, website, funding amount,
employee count, industry, and a brief description.

## Step 2: Enrich with Decision-Maker Contacts via Apollo
For each company from Step 1, use the Apollo publisher to find contacts with
titles containing: "VP Sales", "Head of Sales", "VP Business Development",
"Head of BD", "Revenue Operations", or "RevOps".

For each contact, extract: full name, email (if verified), job title,
LinkedIn URL, and phone number (if available).

## Step 3: Gather Company Intelligence with Firecrawl
For each company, use the Firecrawl publisher to scrape their website's
About page and main product pages. Extract:
- Company positioning/value proposition
- Target customer segments they serve
- Any mentioned technology stack or integrations
- Recent news or announcements

## Step 4: Create Records in Attio CRM
For each fully enriched prospect:

1. Create a Company record in Attio with:
   - Name, website, industry, employee count, funding stage
   - Custom field "Intelligence Notes" with the Firecrawl insights

2. Create Person records for each decision-maker contact, linked to the company:
   - Name, email, job title, LinkedIn URL, phone

3. Set the company's pipeline stage to "Researched"

4. Add a Note to the company record summarizing why they match our ICP

## Output
After processing, write a summary to SerenNotes explaining:
- Total companies added and why each was selected
- Total contacts added
- Any companies skipped and why
- Total Seren API costs incurred
- Your reasoning for ICP match decisions

This gives me a persistent record to review your choices asynchronously.
```

**Estimated Cost Per Prospect (assuming 4 contacts per company):**

| Step | Publisher | Operation | Cost |
|------|-----------|-----------|------|
| 1 | Crunchbase | Company lookup | $0.15 |
| 2 | Apollo | Contact search (returns multiple) | $0.04 |
| 3 | Firecrawl | Website scrape | $0.002 |
| 4 | Attio | Create company + 4 contacts + note | $0.012 |
| — | SerenCron | Nightly execution | $0.0001 |
| — | — | **Total per prospect** | **~$0.20** |

For 500 companies with 2,000 contacts: **~$100 total spend** for a fully enriched pipeline.

### The Results

The agent runs nightly via [SerenCron](https://serendb.com/bestsellers/serencron), continuously building and enriching the prospect database:

- **500+ qualified companies** added to pipeline in first month
- **2,000+ decision-maker contacts** with verified emails
- **Zero manual data entry** by the sales team
- **$0.20 average cost** per fully enriched prospect record

The sales team now spends their time on outreach and closing, not research and data entry.

## How It Works Technically

Attio on Seren MCP uses OAuth for secure, per-user authentication. Here's the architecture:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        ATTIO + SEREN INTEGRATION                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌──────────────┐         ┌──────────────────┐         ┌──────────────┐   │
│   │              │   MCP   │                  │  OAuth  │              │   │
│   │  YOUR AGENT  │ ──────▶ │   SEREN GATEWAY  │ ──────▶ │    ATTIO     │   │
│   │  (Claude,    │         │                  │         │    API v2    │   │
│   │   Cursor)    │ ◀────── │  • Auth          │ ◀────── │              │   │
│   │              │         │  • Billing       │         │  Your Data   │   │
│   └──────────────┘         │  • Rate Limiting │         └──────────────┘   │
│                            └──────────────────┘                             │
│                                                                             │
│   PRICING:                                                                  │
│   • GET operations: $0.0005 per call                                        │
│   • Write operations: $0.002 per call                                       │
│   • Billed to your SerenBucks balance                                       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

Your agent authenticates once through Attio's OAuth flow, and Seren securely manages the tokens. Every API call is metered and billed transparently.

## Available Attio Endpoints

The full Attio v2 API is available through Seren MCP:

| Category | Operations |
|----------|------------|
| **Objects** | List, get custom object configurations |
| **Records** | Create, read, update, delete any record type |
| **Lists** | Manage dynamic lists and list entries |
| **Notes** | Attach notes to records |
| **Tasks** | Create and manage tasks |
| **Comments** | Add comments and threads |
| **Webhooks** | Configure real-time notifications |

## Getting Started

### For Users

1. Visit [serendb.com/bestsellers/attio](https://serendb.com/bestsellers/attio)
2. Connect your Attio workspace via OAuth
3. Fund your SerenBucks balance
4. Start using Attio tools in your AI agent

### For Developers

Here's how to programmatically create the prospect records from our case study using the Seren MCP HTTP API:

```bash
# Step 1: Create the company record in Attio
# This creates "Acme Analytics" - a Series A SaaS company from our Crunchbase search

curl -X POST "https://mcp.serendb.com/api/publishers/attio/v2/objects/companies/records" \
  -H "Authorization: Bearer YOUR_SEREN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "values": {
        "name": [{"value": "Acme Analytics"}],
        "domains": [{"domain": "acmeanalytics.io"}],
        "description": [{"value": "Series A SaaS analytics platform, 120 employees. Tech stack: React, Python, AWS. Targeting mid-market B2B companies."}]
      }
    }
  }'

# Response includes the company record ID for linking contacts
# Cost: $0.002 (write operation)
```

```bash
# Step 2: Create the decision-maker contact, linked to the company
# This adds the VP of Sales we found via Apollo

curl -X POST "https://mcp.serendb.com/api/publishers/attio/v2/objects/people/records" \
  -H "Authorization: Bearer YOUR_SEREN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "values": {
        "name": [{"first_name": "Sarah", "last_name": "Chen"}],
        "email_addresses": [{"email_address": "sarah.chen@acmeanalytics.io"}],
        "job_title": [{"value": "VP of Sales"}],
        "linkedin_url": [{"value": "https://linkedin.com/in/sarachen"}],
        "company": [{"record_id": "COMPANY_RECORD_ID_FROM_STEP_1"}]
      }
    }
  }'

# Cost: $0.002 (write operation)
```

```bash
# Step 3: Add a note with the Firecrawl intelligence
# Attaches the web research to the company record

curl -X POST "https://mcp.serendb.com/api/publishers/attio/v2/notes" \
  -H "Authorization: Bearer YOUR_SEREN_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "parent_object": "companies",
      "parent_record_id": "COMPANY_RECORD_ID_FROM_STEP_1",
      "title": "ICP Match Analysis",
      "content": "Strong ICP fit: Series A ($12M), 120 employees, pure SaaS play. Website shows they target mid-market B2B - likely experiencing our exact pain points. Recently announced expansion into enterprise segment. Priority: HIGH."
    }
  }'

# Cost: $0.002 (write operation)
# Total cost for one fully enriched prospect: ~$0.006 Attio API calls
```

## The Future of CRM is Autonomous

We're entering an era where AI agents don't just assist with CRM tasks—they own entire workflows end-to-end. Sourcing leads, enriching data, scheduling follow-ups, updating deal stages—all happening autonomously while you focus on high-value activities.

Attio's flexible data model combined with Seren's agent-native infrastructure makes this possible today. No custom integrations. No API key management. Just connect and let your agent work.

**Ready to automate your CRM?** Check out [Attio on Seren MCP](https://serendb.com/bestsellers/attio) and start building your autonomous sales pipeline.

---

## Get Started

- **Attio on Seren:** [serendb.com/bestsellers/attio](https://serendb.com/bestsellers/attio)
- **What is Seren?** [serendb.com](https://serendb.com)
- **Full API Docs:** [docs.serendb.com](https://docs.serendb.com)
- **Sign Up Free:** [console.serendb.com](https://console.serendb.com)
- **Attio:** [attio.com](https://attio.com)

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
║              Attio CRM + Seren MCP: Autonomous Sales Pipelines               ║
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

*Published: January 22, 2026*
*Tags: #SerenAI #Attio #CRM #AIAgents #SalesAutomation #MCP #Micropayments #B2BSales #Prospecting*
