# Fitune API and MCP server for ChatGPT and Claude

Fitune publishes no public API documentation or MCP server, according to the Cotlet.dev page checked 16 November 2026. Cotlet.dev connects ChatGPT, Claude and other AI agents to your Fitune studio admin through a hosted MCP server with 32 workflow tools. Writes ask for your approval by default.

## What you can ask

> Book a client into a class.

> Read a client's upcoming and past bookings.

> Create a group class and its schedule.

> Send a payment link for an unpaid invoice after my approval.

> Read studio revenue, refunds and discounts.

> Prepare staff payroll for my review.

## Connect in 2 minutes

Start at https://cotlet.dev/signup?app=fitune, sign in to Fitune in your Cotlet Cloud Browser, and create an API key in the console for editors or scripts. Export the key as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, add connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev using OAuth.

### Claude

Settings → Connectors → Add custom connector, enter `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Fitune tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"fitune":{"command":"uvx","args":["fitune-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t fitune-mcp .
docker run -i --rm -e COTLET_API_KEY fitune-mcp
```

Tools: `fitune_guide`, `fitune_status`, `fitune_open`, `fitune_read`, `fitune_click`, `fitune_type`, `fitune_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install fitune-mcp        # or run it once: uvx fitune-mcp connect
fitune-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 fitune_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 fitune_agent.py status    # is Fitune ready in your Cotlet.dev account
python3 fitune_agent.py guide     # Fitune screens and sign-in link
python3 fitune_agent.py read      # open Fitune, read the page, close the browser
python3 fitune_agent.py tools     # list Cotlet.dev tools
python3 fitune_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Fitune users

**Does Fitune have a public API or MCP server?**

The Cotlet.dev page, checked 16 November 2026, reports no public Fitune API documentation, MCP server or official ChatGPT or Claude connector.

**What can I do with the hosted Fitune integration?**

The live page describes 32 workflow tools covering clients, classes, bookings, memberships, invoices and studio payroll. The local server in this repository exposes seven browser helper tools and forwards calls to Cotlet.dev.

**Is this an official Fitune integration?**

No. Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by Fitune.

## Source

[Fitune integration and all 32 hosted workflow tools](https://cotlet.dev/integrations/fitune-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=fitune)

---

More: https://cotlet.dev/integrations/fitune-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=fitune

Start free: https://cotlet.dev/signup?app=fitune&utm_source=github&utm_medium=readme&utm_campaign=fitune
