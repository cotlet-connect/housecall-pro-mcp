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

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/housecall-pro-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=housecall-pro

Start free: https://cotlet.dev/signup?app=housecall-pro&utm_source=github&utm_medium=readme&utm_campaign=housecall-pro
