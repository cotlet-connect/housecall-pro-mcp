# How to connect Housecall Pro to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your Housecall Pro account.** Housecall Pro's API is only on the MAX plan and needs a developer. Cotlet.dev connects your own Housecall Pro account to any AI agent on any plan, so the AI schedules jobs and sends invoices when you ask in plain words.

## What you can ask

> New job for Tom Baker at 22 Pine St: AC service, book Luis for Monday 9 am.

> Which invoices are unpaid for more than 30 days? Total them per customer.

> Send the estimate for job 1042 to the customer and remind them in 3 days.

> What is on tomorrow's schedule per technician?

> Turn today's finished jobs into invoices. Show me before sending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

## Questions from Housecall Pro users

**Does Housecall Pro have an API?**

Housecall Pro has a public API only on the MAX plan, for admins with a developer. Cotlet.dev works in your Housecall Pro account on any plan, with no code.

**Is there a Housecall Pro ChatGPT integration on GitHub?**

This repo is it: a ready connection of Housecall Pro to ChatGPT, Claude and any AI agent through Cotlet.dev.

**How do I connect Housecall Pro to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Housecall Pro once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Housecall Pro API key.

**Is there a way to use Claude with Housecall Pro?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Housecall Pro account.

**Is there a Housecall Pro MCP server?**

Housecall Pro has no official MCP server. Cotlet.dev is a hosted MCP server that works in Housecall Pro for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

## Find it on

- [Housecall Pro on Cotlet.dev](https://cotlet.dev/integrations/housecall-pro-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro&utm_content=find)
- [PyPI package housecall-pro-mcp](https://pypi.org/project/housecall-pro-mcp/)
- [Glama](https://glama.ai/mcp/servers/cotlet-connect/housecall-pro-mcp)
- [LobeHub](https://lobehub.com/mcp/cotlet-connect-housecall-pro-mcp)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-housecall-pro-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/housecall-pro-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro

Start free: https://cotlet.dev/signup?app=housecall-pro&utm_source=github&utm_medium=readme&utm_campaign=housecall-pro
