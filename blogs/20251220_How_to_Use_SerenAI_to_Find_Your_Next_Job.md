```text
     _______________                    _______________
    |  NETWORKING   |                  |   YOU'RE     |
    |   STRATEGY    |    ╭─────╮       |    HIRED!    |
    |   ████████    |    │ ◠ ◠ │       |   ⭐⭐⭐⭐⭐   |
    |_______________|    │  ▽  │       |_______________|
           │             │ ═══ │              │
           │      $0.88  ╰──┬──╯  AI Agent   │
           │        USDC    │                │
           ▼                ▼                ▼
    ┌──────────────────────────────────────────────┐
    │  🔍 AlphaGrowth → 40 funded companies        │
    │  👤 Perplexity → 15+ hiring managers found   │
    │  📅 Events → ETHDenver, DC Blockchain Summit │
    │  🤖 GPT-5.2 → personalized engagement scripts│
    │                                              │
    │     💳 All paid with USDC micropayments      │
    └──────────────────────────────────────────────┘
```

# How to Use SerenAI to Find Your Next Job

*December 20, 2025 (Updated December 21, 2025)*

If you got laid off this year and you're still looking for work, this guide is for you. SerenAI's x402 MCP Server gives AI agents the power to search databases, scrape websites, and access premium APIs—all paid with USDC micropayments on Base. We're offering **free credits** to help displaced workers find their next opportunity.

> **⚠️ Critical Lesson:** Sending AI-generated resumes is the resume black hole. The real value is using AI to **find hiring managers** and **network into companies**. This guide now emphasizes Prompt 4 (networking strategy) as the primary workflow, with Prompts 1-3 as supporting research.

**2025 is NOT OVER YET.**

---

## Getting Started (5 Minutes)

Before using the prompts below, complete this setup:

### 1. Sign Up for SerenAI
Create your account at [console.serendb.com/signup](https://console.serendb.com/signup) and copy your connection string.

### 2. Join Discord for Support
Get help from the community: [discord.gg/jseg7q4KS7](https://discord.gg/jseg7q4KS7)

### 3. Create a Rabby Wallet
Download from [rabby.io](https://rabby.io/) — this is where you'll receive your free USDC credits for searches.

### 4. Install Cursor IDE
Download from [cursor.com](https://cursor.com/) — the AI-native code editor that works with SerenAI.

### 5. Install Node.js
Visit [nodejs.org](https://nodejs.org/) and download the **LTS version** (the one on the left). Run the installer with default settings. To verify it installed correctly, open your terminal and type `node -v` — you should see `v20.x.x` or higher (e.g., `v20.18.0` or `v22.12.0`).

> **Why Node.js?** Think of it as a background helper that lets Cursor communicate with SerenAI. You install it once and forget about it.

### 6. Install SerenAI MCP Server

**Step 1:** Install the MCP server globally (required to avoid Cursor's 60-second startup timeout):

```bash
npm install -g @serendb/x402-mcp-server
```

**Step 2:** Get your wallet private key from Rabby:

- Open Rabby wallet
- Click on your account → "Export Private Key"
- Copy the key (starts with `0x`)

> **Security note:** This key is used to sign USDC payments on Base. Only fund this wallet with small amounts for job search costs (~$20 USDC is plenty).

**Step 3:** Configure Cursor. Open your MCP config file:

- Mac: `~/.cursor/mcp.json`
- Windows: `%USERPROFILE%\.cursor\mcp.json`

Add this configuration (create the file if it doesn't exist):

```json
{
  "mcpServers": {
    "x402": {
      "command": "x402-mcp-server",
      "args": [],
      "env": {
        "X402_GATEWAY_URL": "https://x402.serendb.com",
        "WALLET_PRIVATE_KEY": "0xYOUR_PRIVATE_KEY_HERE",
        "BASE_RPC_URL": "https://mainnet.base.org"
      }
    }
  }
}
```

Replace `0xYOUR_PRIVATE_KEY_HERE` with your actual Rabby private key.

**Step 4:** Restart Cursor completely (quit and reopen).

**Step 5:** Test it! Open Cursor Chat (`Cmd+L` on Mac, `Ctrl+L` on Windows) and ask: "List available x402 publishers"

If you see a list of data sources like AlphaGrowth, Perplexity, and OpenAI, you're all set!

> **Troubleshooting:** If you see errors, check that Node.js is installed (`node -v`), the MCP server is installed (`x402-mcp-server --help`), and your private key is correctly formatted (starts with `0x` followed by 64 hex characters).

---

## Job Search Prompts by Industry

Below are three powerful prompts tailored to different professional backgrounds. Copy these into Cursor with the SerenAI MCP server enabled.

---

## Prompt 1: Data & AI Strategy Roles in Financial Services

**Best for:** Data governance professionals, AI strategists, enterprise architects, consultants with fintech/banking experience.

```
I'm a Data Governance and AI Strategy professional with experience at major financial
institutions including asset managers, banks, and fintechs. My background includes:
- Enterprise data strategy and governance frameworks
- AI/ML commercialization and implementation
- Regulatory compliance (SEC, DoD, FinTech)
- Core banking integrations (Fiserv, Jack Henry)
- Building RAG systems, data pipelines, and governance dashboards

Help me find my next role. Use the x402 MCP server tools for all searches:

1. **Search for well-funded companies hiring in my space:**
   Use x402 to query AlphaGrowth for fintech and financial services companies that raised
   over $5M in the last 18 months. Show me the company name, funding amount,
   investors, announcement date, and website. Sort by funding amount, top 50.

2. **Find similar companies and job postings:**
   Use x402 to search Exa for companies similar to the top results:
   "fintech companies hiring data governance AI strategy leaders 2025"

   Then use x402 to run Apify to crawl careers pages of the top 15 companies.
   Look for roles containing: "Data", "Governance", "AI Strategy", "Enterprise Architect",
   "Principal", "Director", "VP"

3. **Research each company:**
   Use x402 to query Perplexity Sonar for each company's AI initiatives, recent news,
   and strategic priorities. Identify which are actively building AI/data teams.

4. **Analyze fit with GPT-5.2:**
   Use x402 to call OpenAI GPT-5.2 to analyze each role against my background:
   - Score each opportunity 1-10 for fit
   - Identify transferable skills to highlight
   - Draft a tailored elevator pitch for each company

5. **Output a ranked list:**
   Create a table with: Company, Role Title, Funding Amount, Key Investors,
   Fit Score, Why This Is a Match, and Application Link.

6. **Store all research in my SerenAI database:**
   Save the complete results to my SerenAI database so I can track progress,
   update status as I apply, and reference this research in future sessions.

Focus on companies in Boston, NYC, or remote-friendly. Prioritize Series B+ companies
with dedicated data/AI leadership positions.
```

---

## Prompt 2: Customer Success & Account Management Roles

**Best for:** Customer Success Managers, Account Managers, Product Advocates from SaaS companies.

```
I'm a Customer Success Manager with 4+ years of experience at a major SaaS company.
My background includes:
- Managing enterprise customer relationships
- Driving product adoption and retention
- Cross-functional collaboration with Product and Engineering
- Account management and growth strategies
- Business development in earlier roles

Help me find my next role. Use the x402 MCP server tools for all searches:

1. **Identify high-growth SaaS companies:**
   Use x402 to query AlphaGrowth for SaaS, productivity, enterprise, and collaboration
   companies that raised over $10M since January 2024. Show me company name,
   website, categories, funding amount, investors, and announcement date.
   Sort by funding amount, top 40.

2. **Discover similar opportunities with Exa:**
   Use x402 to search Exa:
   "SaaS companies customer success manager remote friendly Series B 2024 2025"

3. **Research their customer success teams:**
   Use x402 to query Perplexity Sonar for "[Company Name] customer success jobs" and
   "[Company Name] Glassdoor reviews customer success" for each top company.

4. **Scrape job boards at scale:**
   Use x402 to run Apify to scrape LinkedIn job postings matching:
   - "Customer Success Manager" + company names from step 1
   - "Account Manager" + company names from step 1
   - "Client Success" + company names from step 1

5. **Personalize outreach with GPT-5.2:**
   Use x402 to call OpenAI GPT-5.2 to:
   - Analyze each company's product and customer base
   - Draft personalized connection request messages for hiring managers
   - Create a "why I'm interested" talking point for each company

6. **Score and rank opportunities:**
   Create a ranked list based on:
   - Company funding stage (Series B-D preferred for CS team growth)
   - Glassdoor reviews for CS roles
   - Remote-friendly policies
   - Compensation data if available

   Output: Company, Role, Funding, Team Size Estimate, Remote Policy, Match Score, Outreach Draft

7. **Store all research in my SerenAI database:**
   Save the complete results to my SerenAI database so I can track progress,
   update status as I apply, and reference this research in future sessions.

Prioritize companies in Miami, remote-first, or with strong LATAM presence given
my background.
```

---

## Prompt 3: Ad Tech Sales & Partnership Development Roles

**Best for:** Sales leaders, partnership executives, business development professionals in advertising technology, data licensing, and programmatic media.

```
I'm a senior Sales and Partnerships executive with 15+ years in advertising technology.
My background includes:
- Managing $4M+ revenue books at companies like GfK, Samba TV, TubeMogul
- Building data partnership programs with Amazon, Snap, TikTok, LiveRamp
- Launching attribution and measurement products
- Ad tech startup experience (co-founded AI/ML-based mobile analytics company)
- Enterprise sales at Microsoft (127% quota attainment)

Help me find my next VP/Director role. Use the x402 MCP server tools for all searches:

1. **Find funded ad tech and marketing tech companies:**
   Use x402 to query AlphaGrowth for advertising, marketing, media, attribution, programmatic,
   and adtech companies that raised over $15M since January 2024. Show me company
   name, website, description, funding amount, investors, and announcement date.
   Sort by funding amount, top 30.

2. **Expand search with Exa semantic search:**
   Use x402 to search Exa for related companies:
   "adtech martech attribution measurement VP sales partnerships hiring 2025"
   "programmatic advertising data licensing head of partnerships"

3. **Deep-dive on each company's GTM strategy:**
   Use x402 to query Perplexity Sonar to research:
   - "[Company] sales team leadership"
   - "[Company] partnership announcements 2024 2025"
   - "[Company] Series B C hiring plans"

4. **Scrape executive job postings at scale:**
   Use x402 to run Apify to scrape LinkedIn for:
   - VP Sales, VP Partnerships, VP Business Development
   - Head of Data Partnerships, Director of Strategic Accounts
   - Global/Regional Sales Leadership roles

5. **Competitive intelligence:**
   For each promising company, use x402 to query Perplexity Sonar to find:
   - Who their main competitors are
   - Recent customer wins or case studies
   - Executive team background (check for ex-colleagues)

6. **Generate executive briefs with GPT-5.2:**
   Use x402 to call OpenAI GPT-5.2 to create a personalized one-pager for each opportunity:
   - Company overview and funding history
   - Role details and likely compensation range (research comparable roles)
   - Why I'm a fit (map my Amazon, TikTok, Microsoft experience to their needs)
   - 3 custom talking points for outreach based on their recent announcements
   - Draft cold email to the hiring VP/CRO

7. **Store all research in my SerenAI database:**
   Save the complete results to my SerenAI database so I can track progress,
   update status as I apply, and reference this research in future sessions.

Focus on NYC, LA, or fully remote. Prioritize companies where I can leverage
my Amazon, TikTok, or Microsoft relationships.
```

---

## Prompt 4: Identify Hiring Managers & Networking Opportunities

**Best for:** Anyone who wants to bypass the resume black hole and connect directly with decision makers through events and strategic networking.

> **Reality check:** Sending AI-generated resumes doesn't work anymore. Hiring managers are drowning in applications. The new playbook is identifying the right people and meeting them where they are—conferences, meetups, webinars, and industry events.

```
I've identified [X] target companies from my job search. Now I need to find the
HIRING MANAGERS at these companies and discover networking opportunities to
connect with them organically. My target companies are:

[List your top 10-15 companies from the previous prompts]

Help me build a networking strategy. Use the x402 MCP server tools for all searches:

1. **Identify hiring managers and decision makers:**
   Use x402 to run Apify to scrape LinkedIn profiles for:
   - VP/Director/Head of [your function] at each target company
   - People with "hiring" or "building my team" in their recent posts
   - Recruiters and talent acquisition leads at each company

   For each person, capture: Name, Title, Company, LinkedIn URL, Recent Activity

2. **Research each hiring manager:**
   Use x402 to query Perplexity Sonar to find:
   - "[Name] [Company] speaking events 2025"
   - "[Name] podcast interview"
   - "[Name] LinkedIn posts" (topics they care about)
   - "[Name] conference speaker"

   Use x402 to search Exa:
   "hiring manager [function] [company name] speaking conference 2025"

3. **Find events where target companies will be present:**
   Use x402 to query Perplexity Sonar to research:
   - "[Company] conference sponsor 2025"
   - "[Company] events attending 2025"
   - "[Industry] conferences January February March 2025"
   - "[Company] webinar hosting"

   Use x402 to run Apify to scrape event websites for:
   - Speaker lists (match against your hiring manager targets)
   - Sponsor lists (match against your target companies)
   - Attendee companies (if publicly listed)

4. **Map events to hiring managers:**
   Use x402 to call Moonshot Kimi K2 to create a matrix:

   | Event | Date | Location | Target Companies Present | Hiring Managers Speaking | Networking Priority |
   |-------|------|----------|--------------------------|--------------------------|---------------------|

   Score each event by:
   - Number of target companies attending/sponsoring
   - Hiring managers on speaker roster
   - Relevance to your function
   - Cost vs. networking ROI

5. **Create personalized engagement strategies:**
   Use x402 to call Moonshot Kimi K2 to generate for each high-priority hiring manager:

   a) **Pre-event outreach:**
      - Personalized LinkedIn connection request (reference their recent post/talk)
      - NOT "I'm looking for a job" — instead: "Your talk on X resonated because..."

   b) **Event talking points:**
      - 3 thoughtful questions to ask if you meet them
      - 1 insight you can share related to their company's challenges
      - Reference to something specific they've said/written

   c) **Post-event follow-up:**
      - Thank you message that references specific conversation
      - Offer to share a relevant resource or introduction
      - Soft ask: "I'd love to learn more about what you're building"

6. **Build a networking calendar:**
   Output a 90-day calendar with:
   - Events to attend (virtual and in-person)
   - Hiring managers to reach out to (and when)
   - Content to engage with (their posts, podcasts, articles)
   - Follow-up reminders

7. **Track relationships in your SerenAI database:**
   Create a table to track your networking progress:

   | Contact | Company | Role | First Touch | Last Contact | Relationship Status | Next Action |
   |---------|---------|------|-------------|--------------|---------------------|-------------|

8. **Generate strategic pitches for your top 5 targets:**
   Use x402 to call OpenAI GPT-5.2 for this high-value task only. For each of your top 5
   companies, create a "Why I'm the perfect fit" analysis that connects:
   - Their recent funding and strategic direction
   - Specific challenges they're likely facing at this stage
   - Your relevant experience that directly addresses those challenges
   - A compelling narrative for why NOW is the right time

   This is worth $0.15/request because it's the pitch you'll actually use in interviews.

9. **Store all research in my SerenAI database:**
   Save all hiring manager profiles, event calendars, engagement strategies, and pitches
   to my SerenAI database so I can track relationship progress across sessions.

Focus on quality over quantity. Better to have 10 warm relationships with hiring
managers than 100 cold applications.
```

---

### Event Types to Target

| Event Type | Best For | How to Find |
|------------|----------|-------------|
| Industry conferences | Meeting multiple targets at once | Perplexity: "[industry] conference 2025" |
| Company webinars | Learning about their priorities | Apify: scrape company events pages |
| Meetups & local events | Casual networking | Perplexity: "[city] [function] meetup" |
| Podcast appearances | Understanding their thinking | Exa: "[hiring manager name] podcast" |
| LinkedIn Live sessions | Direct engagement via comments | Apify: scrape LinkedIn events |
| Twitter/X Spaces | Real-time conversation | Perplexity: "[company] twitter space" |

---

## Available SerenAI Data Sources

Your searches can leverage these premium data sources:

| Publisher | Data Type | Best For | Price |
|-----------|-----------|----------|-------|
| **AlphaGrowth** | 82K+ crypto/fintech projects, 70K+ VC investments | Finding funded companies | $0.0003/query |
| **OpenAI GPT-5.2** | Frontier reasoning model | Resume analysis, cover letters, interview prep | $0.15/request |
| **Perplexity Sonar** | AI-powered web search with citations | Company research, news, competitive intel | $0.01/request |
| **Exa** | Neural semantic search | Finding similar companies, deep web search | $0.0008/request |
| **Apify** | Web scraping at scale | Scraping careers pages, LinkedIn, job boards | $0.25/run |
| **Firecrawl** | LLM-ready web scraping | Quick page scrapes, markdown conversion | $0.002/page |
| **Moonshot Kimi K2** | 256K context LLM | Long document analysis, code generation | $0.02/request |

---

## Estimated Networking Opportunity Sourcing Costs

Here's what a comprehensive job search costs using SerenAI:

| Usage Scenario | Tool Mix | Estimated Cost |
|----------------|----------|----------------|
| **Single industry prompt** (Prompts 1-3) | 5-10 AlphaGrowth queries, 15 GPT-5.2 requests, 30 Perplexity searches, 10 Exa searches, 5 Apify runs | **$3-5** |
| **Networking strategy** (Prompt 4) | 10 Apify runs, 50 Perplexity searches, 10 Exa searches, 20 Kimi K2 requests, 5 GPT-5.2 pitches | **$4-5** |
| **Full job search** (all 4 prompts) | Combined usage across all tools | **$12-20** |

That's roughly **$15 in USDC** to run a comprehensive AI-powered job search—finding funded companies, scraping job boards, researching hiring managers, and generating personalized outreach for dozens of opportunities.

---

## Case Study: Real Results from a Live Networking Search

[Taariq Lewis](https://linkedin.com/in/taariqlewis), founder of SerenAI and organizer of SF Bitcoin Devs, ran a live networking search targeting VP Sales/BD roles at crypto and AI companies. **Total cost: $0.88 USDC.**

### Target Profile

- **Background:** Founder of crypto startups (SerenAI, VolumeFi, Serica YC S15), organized SF Bitcoin Devs and SF Cryptocurrency Developers, sales leadership experience
- **Target:** VP Sales, Head of BD, VP Partnerships at well-funded crypto/AI companies
- **Location:** SF Bay Area or Remote

### Prompt Snippet Used

```text
Use x402 to query Perplexity Sonar to find hiring managers at these target companies:
- "[Company] head of business development LinkedIn"
- "[Company] VP sales partnerships"
- "[Company] CEO founder Twitter"

For each person found, capture: Name, Title, Company
```

### Hiring Managers Identified (6 Companies)

| Company | Key Contact | Role |
|---------|-------------|------|
| **Monad** | James Harasty | Head of BD |
| **Polymarket** | Sean Li | Head of BD |
| **Kalshi** | Tyson Hackworth | Head of Sales |
| **Farcaster** | Dan Romero | CEO |
| **EigenLayer** | Anshum Khandelwal | Head of BD |
| **MoonPay** | Victor Faramond | Chief Business Officer |

### Networking Events Found

| Event | Date | Location | Why Attend |
|-------|------|----------|------------|
| **ETHDenver 2025** | Feb 23 - Mar 2 | Denver, CO | Largest Ethereum event, all target companies present |
| **DC Blockchain Summit** | Mar 17-18 | Washington, DC | Policy focus, Kalshi/prediction markets relevant |
| **FarCon 2025** | TBD | TBD | Farcaster's flagship event, Dan Romero speaks |

### Podcast Appearances for Warm Outreach

- **Keone Hon (Monad)** — Empire Podcast, multiple appearances discussing L1 scaling
- **Dan Romero (Farcaster)** — Network State Podcast, FarCon keynotes
- **Shayne Coplan (Polymarket)** — 60 Minutes feature, Bloomberg interviews

### Open Roles Found

| Company | Role | Location | Comp | Link |
|---------|------|----------|------|------|
| **Kalshi** | Account Executive | NYC | $160-240K + equity | [jobs.ashbyhq.com/kalshi/10bc8aed...](https://jobs.ashbyhq.com/kalshi/10bc8aed-c653-4d43-b784-20d2aabc9538) |
| **Polymarket** | Institutional Onboarding | Remote | Undisclosed | [jobs.ashbyhq.com/polymarket](https://jobs.ashbyhq.com/polymarket) |
| **Monad** | BD Lead Payment Network | NYC/SF | Undisclosed | [jobs.ashbyhq.com/monad](https://jobs.ashbyhq.com/monad) |
| **Monad** | Head of Institutional BD LATAM | Remote | Undisclosed | [jobs.ashbyhq.com/monad](https://jobs.ashbyhq.com/monad) |

### Cost Breakdown

| Query Type | Tool | Cost |
|------------|------|------|
| Funded company search | AlphaGrowth | $0.001 |
| Hiring manager research (10 queries) | Perplexity Sonar | $0.09 |
| Event discovery (5 queries) | Perplexity Sonar | $0.05 |
| Executive brief generation (3 briefs) | GPT-5.2 | $0.45 |
| Job posting validation | Perplexity Sonar | $0.02 |
| Engagement script generation | GPT-5.2 | $0.26 |
| **Total** | | **$0.88** |

### Key Lesson: Direct Source > AI Search

**Important discovery:** AI search tools (Perplexity, Exa) sometimes miss active job postings. The Kalshi Account Executive role wasn't found by searching—we only discovered it by directly checking their Ashby careers page.

**Best practice:** After identifying target companies, always fetch their careers pages directly:

- **Ashby:** `jobs.ashbyhq.com/[company]`
- **Greenhouse:** `boards.greenhouse.io/[company]`
- **Lever:** `jobs.lever.co/[company]`

Use Firecrawl or Browserbase to scrape these directly instead of relying only on AI search summaries.

### Sample Engagement Script Generated

Here's the actual engagement script GPT-5.2 generated for reaching out to James Harasty (Monad Head of BD):

> **LinkedIn Message:**
> Hi James — I caught your discussion on Monad's payment network strategy at ETHDenver. The framing around "1000x throughput enabling entirely new use cases" resonated with my experience building VolumeFi's DeFi aggregation layer.
>
> I'm particularly interested in how Monad is thinking about institutional BD for the payment rail partnerships you mentioned. Would love to compare notes on the enterprise integration patterns we developed at VolumeFi — we faced similar challenges getting traditional payment providers comfortable with novel settlement mechanics.
>
> Happy to share what worked (and didn't) if useful. No ask here, just genuine interest in the approach.

---

## Why This Works Better Than Applying

The resume black hole is real. Here's why networking-first wins:

| Approach | Response Rate | Time to Response | Quality of Conversation |
|----------|---------------|------------------|------------------------|
| Cold application | 2-5% | 2-4 weeks (if ever) | Form rejection |
| Recruiter referral | 15-20% | 1-2 weeks | Screening call |
| **Warm intro via networking** | 40-60% | 1-3 days | Direct with hiring manager |

By identifying hiring managers and engaging them at events or through thoughtful content engagement, you skip the applicant tracking system entirely. The $0.88 spent on research is 10x more valuable than a perfect resume no human ever reads.

---

## How to Create Your Prompt from Your LinkedIn Experience

**Not in Data/AI, Customer Success, or Ad Tech?** No problem. You can generate a fully customized job search prompt tailored to YOUR industry and experience using Kimi K2 through the x402 MCP server.

Whether you're in real estate, healthcare, education, manufacturing, legal, hospitality, or any other field—here's how to create your own personalized prompt in minutes.

### Step 1: Copy Your LinkedIn Experience

1. Go to your LinkedIn profile
2. Scroll to the **Experience** section
3. Select and copy all your job titles, companies, and descriptions (Ctrl+A, Ctrl+C in that section)
4. Also copy your **Skills** section and any **Certifications**

### Step 2: Ask Kimi K2 to Generate Your Prompt

Open Cursor Chat and paste the following (replacing the bracketed text with your actual LinkedIn content):

```
I need you to create a personalized job search prompt for me using the x402 MCP server.

First, use x402 to call Moonshot Kimi K2 with the following request:

"Based on my LinkedIn experience below, create a comprehensive job search prompt similar to the SerenAI job search prompts. The prompt should:

1. Summarize my professional background in a clear intro paragraph
2. Identify 5-7 specific search queries for AlphaGrowth to find funded companies in my industry
3. Suggest Exa semantic searches tailored to my skills and experience level
4. Include Perplexity Sonar queries to research companies in my field
5. Add Apify scraping tasks for job boards and careers pages in my industry
6. Incorporate GPT-5.2 analysis for role matching and personalized outreach
7. Include the networking strategy (finding hiring managers and events)

Here's my LinkedIn experience:

[PASTE YOUR LINKEDIN EXPERIENCE HERE]

Here are my skills:

[PASTE YOUR LINKEDIN SKILLS HERE]

Generate the complete prompt I can use to run a comprehensive job search in my industry."
```

### Step 3: Review and Run Your Custom Prompt

Kimi K2 will generate a complete job search prompt customized to your background. The prompt will include:

- Industry-specific funding queries (e.g., real estate tech, healthcare IT, legal tech)
- Tailored job title searches matching your experience level
- Company research queries relevant to your sector
- Networking strategies for YOUR industry's conferences and events

**Cost:** About $0.02 for the Kimi K2 call to generate your prompt, then your regular search costs when you run it.

### Example: Real Estate Professional

A real estate professional might get a prompt that includes:
- AlphaGrowth queries for proptech and real estate tech funding
- Exa searches like "commercial real estate technology VP sales 2025"
- Perplexity research on companies like Zillow, Redfin, CoStar, and funded proptech startups
- Event discovery for real estate tech conferences like Blueprint, Inman Connect
- Hiring manager searches targeting VP of Sales, Head of Partnerships at proptech companies

### Why This Works

The three example prompts in this guide are templates—but your career is unique. By feeding your actual LinkedIn experience to Kimi K2, you get:

1. **Industry-specific terminology** that matches how companies in your field describe roles
2. **Correct seniority targeting** based on your actual experience level
3. **Relevant event and conference suggestions** for your industry
4. **Personalized outreach angles** based on your real accomplishments

You don't need to be a developer or prompt engineer. Just copy your LinkedIn, paste it into Cursor, and let Kimi K2 do the work.

---

## Tips for Best Results

1. **Be specific about your background** — The more context you give, the better the AI can match opportunities to your experience.

2. **Iterate on queries** — Start broad, then narrow down based on initial results.

3. **Combine data sources** — Use AlphaGrowth for funded company discovery, Exa for semantic search, Apify for large-scale scraping, Perplexity for research, and GPT-5.2 for personalized analysis.

4. **Save your results** — Ask the AI to output results to your SerenAI database for tracking.

5. **Follow up in Discord** — Share your wins and get help troubleshooting in our community.

---

## Get Your Free Credits

DM [@serenaisoft](https://x.com/serenaisoft) on Twitter/X or join our [Discord](https://discord.gg/jseg7q4KS7) with:
- Your Rabby wallet address
- Brief description of your background
- Which prompt above matches your experience

We'll send you USDC credits on Base to power your job search.

**Let's Go! 2025 is NOT OVER YET.**

---

*SerenAI is the x402 payment gateway enabling AI agents to pay for premium data and APIs with USDC micropayments. Learn more at [serendb.com](https://serendb.com)*
