# Housecall Pro API integration and MCP server: connect Housecall Pro to ChatGPT, Claude and any AI agent

Housecall Pro API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in Housecall Pro account: jobs, the schedule, estimates, invoices, customers, leads and service plans, on the same screens your login opens. Cotlet has 18 ready Housecall Pro tools for AI agents, and the Cloud Browser does the rest. No developer and no Housecall Pro API key needed. Cotlet is independent and is not affiliated with Housecall Pro.

**Does Housecall Pro have an API or an MCP server?** Housecall Pro has a public API, but its help center says it is "available only to Pros on a MAX plan" (checked 22 Nov 2026); an admin generates the key under My Apps, and Housecall Pro gives no API support. Webhooks are MAX only too. Its official Zapier app (Essentials and up) has two triggers, Job Scheduled and Job Finished, and one action, Create New Customer. Housecall Pro has no MCP server and no ChatGPT or Claude app; its own AI Team works inside Housecall Pro.

## What home service contractors ask their AI to do in Housecall Pro

| Job | Ask your AI | Make it repeat |
|---|---|---|
| QuickBooks check | "Compare this month's invoices, payments and deposits with QuickBooks and list what did not match." | Monthly |
| Leads into the pipeline | "Add the leads from my landing page form to the lead pipeline, not only as customers." | Your own endpoint, called from your form tool or Zapier |
| Estimate follow-ups | "List estimates sent over 7 days ago with no answer and draft a follow-up text for each. Send nothing yet." | Weekly |
| Unpaid invoices | "List open invoices past their due date, with the amount due, and draft a reminder for each." | Weekly |
| Which ads bring jobs | "Total last month's job revenue per lead source, Google Ads included." | Monthly |
| Google review requests | "List customers whose job was finished yesterday and draft a review request with my Google review link for each." | Daily |
| Technician commissions | "Total each technician's finished jobs and revenue last month for commissions." | Monthly |
| Hours for payroll | "Total last week's tracked hours per employee for payroll." | Weekly |
| Service plan visits | "List service plan visits due next month that are not on the schedule yet." | Monthly |
| Jobs in a spreadsheet | "Put each day's jobs with technician, status and total in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Import from your old system | "Import the customers in this spreadsheet from Jobber, with their tags. Show me before you save." | Once |
| Your history out | "Export all customers, jobs, estimates and invoices with line items and notes to a spreadsheet." | Before you cancel |

Your AI asks you before it texts or emails a customer, sends an estimate or invoice, moves a job or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own Housecall Pro API, built with Cotlet

Housecall Pro's API and webhooks come only with the MAX plan. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, GoHighLevel, QuickBooks, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `jobs-for-day` | time, technician, customer, address and job for each scheduled job |
| `open-estimates` | estimates with no answer, with customer, total and days since sent |
| `past-due-invoices` | customer, amount due and last sent date of each invoice past its due date |
| `revenue-by-source` | job revenue per lead source for a period |
| `tech-revenue` | finished jobs and revenue per technician for a period |
| `service-plan-due` | service plan visits due and not yet scheduled |
| `new-lead` | adds a lead to your lead pipeline from your form (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/jobs-for-day \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=housecall-pro

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to Housecall Pro once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Housecall Pro tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"housecall-pro":{"command":"uvx","args":["housecall-pro-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t housecall-pro-mcp .
docker run -i --rm -e COTLET_API_KEY housecall-pro-mcp
```

Tools: `housecall_pro_guide`, `housecall_pro_status`, `housecall_pro_open`, `housecall_pro_read`, `housecall_pro_click`, `housecall_pro_type`, `housecall_pro_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install housecall-pro-mcp        # or run it once: uvx housecall-pro-mcp connect
housecall-pro-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 housecall_pro_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 housecall_pro_agent.py status    # is Housecall Pro ready in your Cotlet.dev account
python3 housecall_pro_agent.py guide     # Housecall Pro screens and sign-in link
python3 housecall_pro_agent.py read      # open Housecall Pro, read the page, close the browser
python3 housecall_pro_agent.py tools     # list Cotlet.dev tools
python3 housecall_pro_agent.py serve     # start the stdio MCP server (same as no argument)
```

## What you can ask your AI to do in Housecall Pro

These are real jobs Housecall Pro users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in Housecall Pro account.

### Leads and estimates

#### How do I send leads from my website or Wix form into the Housecall Pro lead pipeline, not just as a new customer?

Yes. Housecall Pro's Zapier app has one action, Create New Customer, so form leads do not land in the pipeline. With Cotlet, your agent adds each lead to the Pipeline in your signed-in account with its source and notes. Nothing changes until you say yes.

Ask your agent: "Add the new leads from my website form to the Housecall Pro lead pipeline with name, phone, address, job type and lead source. Show me the list first."

As an API endpoint: Yes. Do the lead entry once with your agent in Cotlet and save it as an endpoint, then call it from your form tool, n8n, Make or Zapier on every new submission.

#### Can ChatGPT or Claude write follow-up texts for my open estimates in Housecall Pro and send them for me?

Yes. Housecall Pro has a Pipeline for estimates. With Cotlet, ChatGPT or Claude lists the estimates still waiting for an answer and drafts a short text for each customer. Nothing is sent until you say yes.

Ask your agent: "List my open Housecall Pro estimates older than five days and draft a short, friendly follow-up text for each customer. Do not send anything yet."

As an API endpoint: Yes. Save the open-estimates check as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier to get the drafts in your inbox.

#### How do I make Housecall Pro convert approved estimates into jobs and book the first visit without me touching it?

Partly. Housecall Pro's Pipeline tracks estimates through stages. With Cotlet, your agent finds approved estimates in your signed-in account, creates the job and proposes the first visit on the Schedule. Nothing changes until you say yes.

Ask your agent: "Find approved Housecall Pro estimates with no job yet, create the job for each and propose the first visit time. Show me the list before booking."

As an API endpoint: Yes. Save the approved-estimates check as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier.

#### Can AI answer my phone and book jobs straight into Housecall Pro? Is the built-in CSR AI enough or do I need another tool?

Not on its own. Housecall Pro's own CSR AI is a paid add-on that handles calls. Cotlet does not answer phones. It helps afterwards: your agent reads the new job requests in Housecall Pro, fills in missing details and drafts replies. Nothing is sent until you say yes.

Ask your agent: "Read this week's new job requests in Housecall Pro, list the ones with missing address or job details and draft a reply asking for them."

#### How do I connect Housecall Pro to a call tracking tool like CallRail or my phone system?

Partly. Housecall Pro keeps a lead source on each job. With Cotlet, your agent matches the calls listed in your tracking tool, signed in in the same Cloud Browser, to jobs in Housecall Pro and fills in the lead source. Nothing changes until you say yes.

Ask your agent: "Match last week's CallRail calls to Housecall Pro jobs by phone number and propose the lead source for each job. Show me before saving."

As an API endpoint: Yes. Save the matching as an endpoint in Cotlet and call it every night from n8n, Make or Zapier.

### Marketing and reports

#### How do I connect Housecall Pro to Google Ads and GA4 so I can track which ad produced which job?

Partly. Housecall Pro has lead sources and a Marketing dashboard with an ROI table. With Cotlet, your agent reads your jobs and revenue by source in Housecall Pro and lines them up with spend from Google Ads, signed in in the same Cloud Browser.

Ask your agent: "Read last month's Housecall Pro jobs and revenue by lead source and compare them with my Google Ads spend per campaign. Give me one table."

As an API endpoint: Yes. Save the monthly comparison as an endpoint in Cotlet and call it from n8n, Make or Zapier to fill a Google Sheet.

#### How do I pull a report of jobs by lead source in Housecall Pro to see which marketing works?

Yes. Housecall Pro has reports you can export from Reporting. With Cotlet, your agent opens your signed-in account, reads jobs and revenue by lead source for any period and gives you one table with the best and worst sources.

Ask your agent: "Give me Housecall Pro jobs and invoiced revenue by lead source for the last 90 days, with the share of each source."

As an API endpoint: Yes. Save the report as an endpoint in Cotlet and call it every month from n8n, Make or Zapier.

#### Can Claude look at my Housecall Pro jobs and tell me which technicians and which lead sources make me money?

Yes. Housecall Pro's built-in Analyst AI answers some questions inside the product. With Cotlet, Claude reads jobs, invoices and technician names in your signed-in account and ranks technicians and lead sources by revenue per job.

Ask your agent: "Read the last 90 days of Housecall Pro jobs and rank technicians and lead sources by revenue per job. Point out the weakest ones."

As an API endpoint: Yes. Save the ranking as an endpoint in Cotlet and call it weekly from n8n, Make or Zapier to email it to you.

#### How do I get my Housecall Pro jobs into Google Sheets automatically every day?

Yes. Housecall Pro's Zapier app only reports scheduled and finished jobs. With Cotlet, your agent reads the day's jobs in your signed-in account and adds one row per job to Google Sheets, signed in in the same Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Read today's Housecall Pro jobs and add a row per job to my Google Sheet with customer, address, technician, status and total."

As an API endpoint: Yes. Save the day's job read as an endpoint in Cotlet and call it every evening from n8n, Make or Zapier.

#### How do I connect Housecall Pro to GoHighLevel so new leads and booked jobs sync both ways?

Partly. Housecall Pro's Zapier app only creates customers. With Cotlet, your agent copies new leads from GoHighLevel into Housecall Pro and booked jobs back, both signed in in the same Cloud Browser. Nothing changes until you say yes.

Ask your agent: "Compare new GoHighLevel contacts with Housecall Pro customers and list the ones missing in Housecall Pro. Show me before adding them."

As an API endpoint: Yes. Save the sync as an endpoint in Cotlet and call it every hour from n8n, Make or Zapier.

### QuickBooks and payments

#### How do I connect Housecall Pro to QuickBooks Online without getting duplicate invoices?

Partly. Housecall Pro has a QuickBooks Online sync with sync alerts and a help article on duplicate invoice errors. With Cotlet, your agent finds invoices that exist twice in QuickBooks or failed to sync and lists them for you. Nothing changes until you say yes.

Ask your agent: "Compare this month's Housecall Pro invoices with QuickBooks Online, list the duplicates and the ones that failed to sync. Do not change anything."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it every week from n8n, Make or Zapier.

#### Can AI reconcile my Housecall Pro payments and invoices against QuickBooks so my bookkeeper stops doing it by hand?

Yes. With Cotlet, your agent reads invoices, payments and deposits in Housecall Pro and the matching entries in QuickBooks, signed in in the same Cloud Browser. It lists what did not match, so your bookkeeper fixes only those.

Ask your agent: "Compare this month's Housecall Pro invoices, payments and deposits with QuickBooks and list every difference with the amount and customer."

As an API endpoint: Yes. Save the comparison as an endpoint in Cotlet and call it on the first of each month from n8n, Make or Zapier.

#### Can AI find out why my Housecall Pro service plan payments show a $0 invoice on each visit?

Partly. Cotlet cannot change how Housecall Pro bills. Your agent opens the service plan, its visits and invoices in your signed-in account, lists which visits carry a $0 invoice and reports how each plan is set up, so you or support can fix it.

Ask your agent: "Open my Housecall Pro service plans, list each plan's visits and invoice amounts and tell me which visits show a $0 invoice."

#### How do I turn tracked technician hours in Housecall Pro into payroll automatically, or export them to Gusto or QuickBooks?

Partly. Housecall Pro has time tracking and a payroll feature under Time & Attendance. With Cotlet, your agent reads each technician's hours for the pay period and gives you a table, or enters them in Gusto or QuickBooks, signed in in the same Cloud Browser. Nothing changes until you say yes.

Ask your agent: "Read each technician's tracked hours in Housecall Pro for last week and give me a table with regular and overtime hours for payroll."

As an API endpoint: Yes. Save the hours read as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

### Customers and your data

#### How do I export all my customers, jobs, invoices and estimates from Housecall Pro before I leave, including notes and job photos?

Yes. Housecall Pro's own export is CSV lists sent by email. With Cotlet, your agent opens each job, estimate and invoice in your signed-in account and collects line items, notes and photo links into one spreadsheet.

Ask your agent: "Read all my Housecall Pro customers and their jobs, invoices and estimates with line items and notes and give me one spreadsheet."

As an API endpoint: Yes. Save the export as an endpoint in Cotlet and call it every month from n8n, Make or Zapier to keep a backup in Google Sheets.

#### Why does Housecall Pro say '0 imported' when I import my Jobber customers from CSV, and how do I fix the file?

Partly. Housecall Pro has an import wizard for customers and jobs with its own column rules. With Cotlet, your agent compares your Jobber CSV with those rules, fixes the columns and adds the customers in your signed-in account. Nothing changes until you say yes.

Ask your agent: "Check my Jobber customer CSV against the Housecall Pro import columns, fix the file and then add the customers. Show me the fixed rows first."

#### Can AI build my Housecall Pro price book from my supplier price list and my old invoices?

Yes. Housecall Pro has a price book with import and export. With Cotlet, your agent reads your supplier list and past invoices, builds the services and materials with prices and adds them to the price book. Nothing changes until you say yes.

Ask your agent: "Read my supplier price list and last year's invoices and propose Housecall Pro price book entries with name, cost and price. Show me before adding."

As an API endpoint: Yes. Save the price book update as an endpoint in Cotlet and call it from n8n, Make or Zapier when a supplier list changes.

### Scheduling and customer messages

#### How do I create Housecall Pro jobs automatically from a Google Calendar or Airbnb booking feed?

Yes. With Cotlet, your agent reads upcoming events in Google Calendar, signed in in the same Cloud Browser, and creates the matching jobs in your signed-in Housecall Pro with address, date and notes. Nothing changes until you say yes.

Ask your agent: "Read next week's Google Calendar events tagged cleaning and create a Housecall Pro job for each with address, date and notes. Show me first."

As an API endpoint: Yes. Save the job creation as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier.

#### How do I automatically ask every Housecall Pro customer for a Google review after a job, including customers without a Gmail account?

Yes. Housecall Pro has automatic review requests under Reviews. With Cotlet, your agent lists finished jobs that got no request and drafts a text or email review link for each. Nothing is sent until you say yes.

Ask your agent: "List Housecall Pro jobs finished this week with no review request and draft a short message with my Google review link for each customer."

As an API endpoint: Yes. Turn the check into an endpoint in Cotlet and call it nightly from n8n, Make or Zapier.

#### How do I send 'on my way' texts and appointment reminders automatically in Housecall Pro?

Partly. Housecall Pro sends SMS job reminders and has automated text campaigns on its own. With Cotlet, your agent checks tomorrow's schedule, finds jobs with no reminder or opt-in and drafts the message. Nothing is sent until you say yes.

Ask your agent: "Check tomorrow's Housecall Pro schedule, list customers who have no reminder set up and draft a short reminder text for each."

## Questions from Housecall Pro contractors
**Does Housecall Pro have an API, and which plan do I need?**

Yes, on the MAX plan only, per its help center (checked 22 Nov 2026). An admin generates the key, and Housecall Pro tells you to hire a developer. With Cotlet you build your own endpoints on any plan, in your signed-in account.

**Can Zapier add leads to the Housecall Pro lead pipeline?**

Housecall Pro's Zapier app has one action, Create New Customer, so form leads do not land in the lead pipeline. Build a `new-lead` endpoint with Cotlet and call it from Zapier, Make or your form tool.

**Is there an official Housecall Pro MCP server or ChatGPT app?**

No. Housecall Pro's AI Team works inside Housecall Pro, and Zapier's Housecall Pro MCP only creates customers. Cotlet is unofficial, has 18 ready Housecall Pro tools for AI agents and works in the ChatGPT and Claude apps and any MCP client.

**Can AI reconcile Housecall Pro payments and invoices against QuickBooks?**

Yes. Your AI compares invoices, payments and deposits in Housecall Pro with QuickBooks and lists what did not match, so your bookkeeper fixes only those.

**How do I export all my customers, jobs, invoices and estimates before I leave?**

Your AI exports them with line items and notes from the screens your login opens, into one spreadsheet, so you keep your history.

**Can ChatGPT or Claude follow up on my open estimates?**

Yes. It lists estimates with no answer and drafts a follow-up text for each. They go out only after your OK.

**How do I ask every customer for a Google review after a job?**

Your AI drafts each request with your own Google review link, so it works for every customer, and sends it after your OK.

**How do I see which lead sources and ads make me money?**

Ask your AI to total job revenue per lead source each month, or build a `revenue-by-source` endpoint and call it from Google Sheets.

**Can I get webhooks without the MAX plan?**

Webhooks are MAX only. With Cotlet, call your own endpoints, for example `jobs-for-day`, on a schedule from Zapier, Make or n8n instead.

**How do I connect Housecall Pro to GoHighLevel?**

Build `new-lead` and `jobs-for-day` endpoints with Cotlet and call them from GoHighLevel workflows, so leads go in and booked jobs come back.

## Find it on

- [Housecall Pro MCP server on Cotlet.dev](https://cotlet.dev/integrations/housecall-pro-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro&utm_content=find)
- [Housecall Pro API with Cotlet](https://cotlet.dev/integrations/api/housecall-pro?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro&utm_content=find)
- [Does Housecall Pro have an API or MCP server?](https://cotlet.dev/api-mcp-status/housecall-pro?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro&utm_content=find)
- [PyPI package housecall-pro-mcp](https://pypi.org/project/housecall-pro-mcp/)

## More home service apps on Cotlet

- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Tradify MCP](https://github.com/cotlet-connect/tradify-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
