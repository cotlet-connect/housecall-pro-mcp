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
