# How to Use OpenClaw and SerenDesktop to Manage Your Wells Fargo Bank Accounts

```
+------------------------------------------------------------------------------------------------+
|                                                                                                |
|   __        __   _ _      _____                          ______                               |
|   \ \      / /__| | |___ |  ___|_ _ _ __ __ _  ___     / ___/___  _ __ ___                  |
|    \ \ /\ / / _ \ | / __|| |_ / _` | '__/ _` |/ _ \   | |   / _ \| '__/ _ \                 |
|     \ V  V /  __/ | \__ \|  _| (_| | | | (_| | (_) |  | |__| (_) | | |  __/                 |
|      \_/\_/ \___|_|_|___/|_|  \__,_|_|  \__, |\___/    \____\___/|_|  \___|                 |
|                                          |___/                                                 |
|                                                                                                |
|               OPENCLAW + SERENDESKTOP + SEREN SKILLS + SERENDB                               |
|                                                                                                |
+------------------------------------------------------------------------------------------------+
```

**Published**: March 2, 2026  
**Reading Time**: 9 minutes  
**Author**: Taariq Lewis, SerenAI

---

If you check your Wells Fargo accounts every week, download statements manually, and then still need to organize spending, you are doing double work.

The better flow is to let a skill do the repetitive steps: open the right page, pull statements, extract transactions, sync to a database, and keep local PDF records for audit and reconciliation.

In this guide, you will set up the **Wells Fargo Bank Statements** skill and run it from both SerenDesktop and OpenClaw. You will also see how to turn your statement data into a dashboard-ready dataset.

Useful links:
- Main site: [https://serendb.com/](https://serendb.com/)
- Skills directory: [https://serendb.com/skills](https://serendb.com/skills)
- Install page: [https://serendb.com/install](https://serendb.com/install)

## 1) Install SerenDesktop

Start with SerenDesktop so you have local browser automation, skill management, and database integration in one place.

1. Open [https://serendb.com/install](https://serendb.com/install).
2. Download the installer for macOS, Linux, or Windows.
3. Install and launch SerenDesktop.
4. Sign in to your Seren account.
5. Connect your preferred model provider (Claude, OpenAI/Codex, or another configured provider).

At this point, SerenDesktop can run skills and access the local Playwright browser automation bridge.

```
+---------------------------------------------------------------+
|  SERENDESKTOP BOOT FLOW                                       |
|                                                               |
|  Download -> Install -> Sign In -> Connect Model -> Ready     |
|                                                               |
|  [ skills ] [ browser ] [ serendb ] [ artifacts ]             |
+---------------------------------------------------------------+
```

## 2) Install the Wells Fargo Skill in SerenDesktop and OpenClaw

The skill name is:
- **Wells Fargo Bank Statements**
- repo path: `wellsfargo/bank-statement-retrieval`

### SerenDesktop skill install

In SerenDesktop:
1. Open the Skills panel.
2. Search `Wells Fargo Bank Statements`.
3. Add it to your thread/workspace.
4. Confirm the slug/path points to `wellsfargo/bank-statement-retrieval`.

### OpenClaw skill install with `npx`

If you use OpenClaw, use `npx` so you do not need a separate global install step:

```bash
npx -y clawhub install wellsfargo/bank-statement-retrieval
```

Optional verification:

```bash
openclaw skills list
openclaw skills info bank-statement-retrieval
```

If `openclaw` is not installed yet:

```bash
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

## 3) Activate the Skill in SerenDesktop

Installing a skill and activating it are separate steps.

1. In your SerenDesktop chat/thread, click the installed skill chip.
2. Confirm it shows as active for this thread.
3. Start a request such as:
   - “Download my latest Wells Fargo statements and sync to SerenDB.”
   - “Run Wells Fargo Bank Statements for the last 6 months.”

When activated, the skill runtime handles browser orchestration, statement indexing, PDF parsing, categorization, and database sync.

## 4) Run the Skill, Choose Browser, Login, and Hand Off to the Agent

This is where most users save the most time.

The updated skill behavior is:
- It opens directly to `https://wellsfargo.com/` so you do not type the URL.
- After login handoff, it auto-detects the **Accounts** navigation context and routes to **Statements & Documents**.
- It supports both Firefox and Chrome-family browser paths with browser-specific fallback logic.

Typical run flow:
1. Choose browser (`Firefox` or `Google Chrome`).
2. Skill opens Wells Fargo.
3. You complete sign-in (and OTP/passkey if required).
4. You press Enter to hand control back.
5. Agent moves to statements/documents automatically.
6. Agent downloads statement PDFs for the configured months.

```
+--------------------------------------------------------------------------------+
|  HUMAN + AGENT HANDOFF                                                         |
|                                                                                |
|  [Agent opens wellsfargo.com] -> [You sign in] -> [Press Enter]               |
|             -> [Agent navigates Accounts -> Statements] -> [PDF downloads]     |
|                                                                                |
+--------------------------------------------------------------------------------+
```

You can explicitly run browser variants in CLI mode:

```bash
# Chrome path
python3 scripts/run.py --mode read-only --auth-method manual --browser-app "Google Chrome" --browser-type chrome --months 3 --out artifacts/wellsfargo

# Firefox path
python3 scripts/run.py --mode read-only --auth-method manual --browser-app "Firefox" --browser-type moz-firefox --months 3 --out artifacts/wellsfargo
```

## 5) How Data Extraction Works: SerenDB + Local PDFs

After document retrieval, the skill performs a structured extraction pipeline:

1. Statement metadata is captured (account mask, period, hash, file size).
2. Transaction lines are parsed from PDFs.
3. Categories are applied using rules-first plus model fallback.
4. Records are upserted into SerenDB tables.
5. Local artifacts remain available for audit and replay.

What gets saved locally:
- PDF files in `artifacts/wellsfargo/pdfs/`
- machine-readable reports in `artifacts/wellsfargo/reports/`
- state checkpoints in `artifacts/wellsfargo/state/`

What gets saved to SerenDB:
- run-level metadata
- statement file metadata
- normalized transactions
- category mappings
- monthly summaries

This gives you both compliance-friendly local records and query-ready structured data in your database.

## 6) Build a Dashboard from Your Banking Data

Once your transactions are in SerenDB, you can ask the agent for analytics instead of reading raw statements.

Examples:
- “Show monthly spending by category for the last 12 months.”
- “Build a cashflow dashboard with income vs expense trendlines.”
- “Flag recurring charges over $100 and show month-over-month deltas.”
- “Create a business vs personal spend breakdown for tax prep.”

```
+--------------------------------------------------------------------------------+
|  FROM PDF CHAOS TO QUERYABLE FINANCE                                           |
|                                                                                |
|  STATEMENTS -> EXTRACTION -> SERENDB -> SQL/AI QUERIES -> DASHBOARD           |
|                                                                                |
|  [pdf] [pdf] [pdf]   ->   [transactions table]   ->   [insights + charts]     |
+--------------------------------------------------------------------------------+
```

A practical dashboard starter layout:
- KPI row: total inflow, total outflow, net cashflow, average monthly burn.
- Category stack: top spend categories by month.
- Account trend: ending balance trend by account.
- Recurring section: subscriptions, utilities, payroll.
- Exceptions: unusual transactions and large one-offs.

Because the skill preserves deterministic metadata (hashes, period dates, masked account refs), your dashboard numbers can be traced back to source documents when needed.

## 7) Join the Seren Community for Support and New Financial Skills

If you want faster iteration, request enhancements in the Seren community and skill ecosystem.

Where to start:
- Browse existing capabilities: [https://serendb.com/skills](https://serendb.com/skills)
- Follow platform updates: [https://serendb.com/](https://serendb.com/)
- Install desktop updates and runtime improvements: [https://serendb.com/install](https://serendb.com/install)

Good requests to submit:
- institution-specific reconciliation templates
- tax-category mappings by jurisdiction
- fraud/anomaly detection add-ons
- monthly close workflows and report exports

The most useful pattern is simple: automate ingestion once, then reuse the same clean dataset for budgeting, forecasting, tax prep, and management reporting.

## Final Checklist

- SerenDesktop installed and authenticated.
- Wells Fargo Bank Statements skill installed and activated.
- Browser path tested in Chrome and Firefox modes.
- Statements downloaded locally as PDFs.
- Transactions synced to SerenDB.
- First dashboard query completed.

You now have a repeatable system: no repeated clicking, no manual statement downloads, and no rebuilding reports from scratch every month.

```
+--------------------------------------------------------------------------------+
|  SERENAI FOOTER                                                                |
|                                                                                |
|   Keep the PDFs local. Keep the data structured. Keep the workflow repeatable. |
|                                                                                |
|   Wells Fargo Bank Statements Skill -> Reliable Financial Ops                  |
|                                                                                |
+--------------------------------------------------------------------------------+
```
