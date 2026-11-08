---
title: Smithery Shutdown: Official MCP URLs to Switch To (2026)
date: 2026-11-08T06:53:48.369Z
---

![A powered-off server with cut cables beside a laptop whose fiber-optic beams connect directly to six glowing service nodes, illustrating the switch from Smithery proxy URLs to official MCP server endpoints](https://www.mcpinstall.com/api/r2-image?fullpath=blog%2Fsmithery-shutdown-official-mcp-urls-hero.jpg)

**Smithery shut down its hosted MCP connections on October 31, 2026. Any server URL on `server.smithery.ai` or `run.tools` has stopped working. To fix it, replace each Smithery URL with the vendor's official MCP endpoint. The table below lists 20 of them, each tested on November 8, 2026.**

Smithery was acquired by Arcade.dev on August 5, 2026. Its co-founder posted the shutdown notice on [smithery.ai/future](https://www.smithery.ai/future): smithery.ai stays online as a public directory, but it no longer proxies connections. If your Claude Code, Cursor or Claude Desktop setup still points at a Smithery URL, those tools have been failing since November 1.

## What stopped working on October 31

According to Smithery's own notice, these ended on October 31, 2026:

| Stopped | What it means for you |
|---|---|
| Server URLs on `run.tools` and `server.smithery.ai` | Every MCP config that uses them now fails to connect |
| Smithery API, Connect and Toolbox | Apps that managed MCP connections through Smithery need a replacement |
| Smithery accounts and API keys | Keys in your config files are dead; remove them |
| Smithery CLI and SDKs | Current versions are deprecated; `smithery install` flows no longer work |

Paid subscribers are being refunded for every charge since August 5, and subscriptions are cancelled.

## What still works

- **smithery.ai as a directory.** You can still browse servers there. It is now "powered by Arcade.dev".
- **Direct URLs on verified servers.** For a curated set, registry pages now show the vendor's direct URL (Smithery's example: `mcp.linear.app/mcp` instead of `linear.run.tools` or `server.smithery.ai/linear`).
- **Anything you run locally.** Servers you started with `npx`, `uvx` or Docker never went through Smithery and are unaffected.

## Official remote MCP URLs for 20 popular servers

We sent a standard MCP `initialize` request to each endpoint on **November 8, 2026**. A `401` means the server is online and wants you to sign in (OAuth or an API key), which is the normal response for services that act on your account. A `200` means it accepted an unauthenticated session.

| Service | Official MCP URL | Test result (2026-11-08) | Directory page |
|---|---|---|---|
| Linear | `https://mcp.linear.app/mcp` | 401, sign-in required | — |
| Notion | `https://mcp.notion.com/mcp` | 401, sign-in required | [notion-mcp-server](https://www.mcpinstall.com/mcp-server/8042-notion-mcp-server) |
| GitHub | `https://api.githubcopilot.com/mcp/` | 401, sign-in required | [GitHub](https://www.mcpinstall.com/mcp-server/863-github) |
| Atlassian (Jira, Confluence) | `https://mcp.atlassian.com/v1/mcp` | 401, sign-in required | [Atlassian](https://www.mcpinstall.com/mcp-server/1048-atlassian) |
| Asana | `https://mcp.asana.com/v2/mcp` | 401, sign-in required | — |
| Figma | `https://mcp.figma.com/mcp` | 401, sign-in required | — |
| Canva | `https://mcp.canva.com/mcp` | 401, sign-in required | — |
| Intercom | `https://mcp.intercom.com/mcp` | 401, sign-in required | — |
| monday.com | `https://mcp.monday.com/mcp` | 401, sign-in required | [Monday.com](https://www.mcpinstall.com/mcp-server/16357-mcp) |
| Box | `https://mcp.box.com` | 401, sign-in required | [Box](https://www.mcpinstall.com/mcp-server/839-box) |
| Stripe | `https://mcp.stripe.com` | 401, sign-in required | [Stripe](https://www.mcpinstall.com/mcp-server/913-stripe) |
| Sentry | `https://mcp.sentry.dev/mcp` | 401, sign-in required | [Sentry](https://www.mcpinstall.com/mcp-server/909-sentry) |
| Supabase | `https://mcp.supabase.com/mcp` | 401, sign-in required | [Supabase](https://www.mcpinstall.com/mcp-server/911-supabase) |
| Vercel | `https://mcp.vercel.com` | 401, sign-in required | — |
| PayPal | `https://mcp.paypal.com/mcp` | 401, sign-in required | [PayPal](https://www.mcpinstall.com/mcp-server/896-paypal) |
| Tavily | `https://mcp.tavily.com/mcp/?tavilyApiKey=<your-key>` | 401 without a key | [Tavily](https://www.mcpinstall.com/mcp-server/914-tavily) |
| Exa | `https://mcp.exa.ai/mcp` | 200, open | [Exa](https://www.mcpinstall.com/mcp-server/857-exa) |
| Context7 | `https://mcp.context7.com/mcp` | 200, open | [context7-mcp](https://www.mcpinstall.com/mcp-server/8214-context7-mcp) |
| Hugging Face | `https://huggingface.co/mcp` | 200, open | — |
| Cloudflare Docs | `https://docs.mcp.cloudflare.com/mcp` | 200, open | [Cloudflare](https://www.mcpinstall.com/mcp-server/847-cloudflare) |

Two services are left out on purpose. Zapier issues each user a personal MCP URL from its dashboard, and Firecrawl puts your API key in the URL path. For both, copy the URL from your own account rather than from a list.

Endpoints change. Before you paste one into a production config, check the vendor's documentation; the directory pages above link to each project's repository.

## How to update your config

The fix is the same everywhere: delete the Smithery entry and add the official URL. Here is what that looks like in the three most common clients, using Linear as the example.

### Claude Code

Remove the old server and add the new one over HTTP:

```bash
claude mcp remove linear
claude mcp add --transport http linear https://mcp.linear.app/mcp
```

Then run `/mcp` inside Claude Code to sign in. Servers that return `401` will open a browser for OAuth.

### Cursor

Edit `~/.cursor/mcp.json` (or `.cursor/mcp.json` in your project):

```json
// Before: dead since October 31
{ "mcpServers": { "linear": { "url": "https://server.smithery.ai/linear/mcp?api_key=..." } } }

// After
{ "mcpServers": { "linear": { "url": "https://mcp.linear.app/mcp" } } }
```

Delete the Smithery `api_key` from the file at the same time; it no longer does anything.

### Claude Desktop

On a plan with custom connectors, open **Settings → Connectors → Add custom connector** and paste the official URL. Otherwise, bridge the remote server through `mcp-remote` in `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "linear": { "command": "npx", "args": ["-y", "mcp-remote", "https://mcp.linear.app/mcp"] }
  }
}
```

Restart Claude Desktop after editing the file.

## What if the vendor has no hosted endpoint?

Many community servers were only ever reachable through Smithery's proxy. For those, run the server yourself:

1. Find it in the [MCP server directory](https://www.mcpinstall.com/browse) and open its GitHub repository.
2. Follow the README's local install, usually `npx -y <package>` or `uvx <package>`, and add it to your client as a `stdio` server.
3. Put any API keys in environment variables, not in the URL.

Local `stdio` servers run on your machine, so they also keep working if any hosting company changes its plans again.

## When an official URL is not the answer

- **You served MCP to your own users through Smithery Connect.** Swapping URLs in a config file does not replace a connection broker. You need another hosted gateway or your own OAuth handling. Smithery's notice points to Arcade for this, and its co-founder offers to help pick a replacement "even if it isn't Arcade".
- **The vendor's endpoint does less than the community server you used.** Official servers often expose a narrower tool set. Compare the tool list before you migrate a workflow that depends on a specific tool.
- **You need it to work offline or inside a locked-down network.** Use a local `stdio` server instead of any remote URL.

## FAQ

### Is Smithery shutting down completely?

No. smithery.ai remains a public directory of MCP servers, now run by Arcade.dev. What ended on October 31, 2026 is the connection layer: hosted server URLs, the API, accounts, API keys, the CLI and the SDKs.

### Why do my MCP tools fail with a connection error since November 1?

Your config most likely still points at a `server.smithery.ai` or `run.tools` URL. Those stopped responding on October 31. Replace them with the vendor's official URL from the table above.

### What does a 401 from an official MCP URL mean?

It means the server is online and needs you to authenticate. Clients like Claude Code and Cursor handle this by opening an OAuth sign-in in your browser. Services that act on your data (Linear, Notion, GitHub, Stripe) all require it.

### Where can I find MCP servers now that Smithery's hosting is gone?

Directories still list them; you just connect directly. [mcpinstall.com](https://www.mcpinstall.com/browse) indexes more than 23,000 MCP servers with links to each repository, and our guide on [how to use MCP](https://www.mcpinstall.com/blog/how-to-use-mcp) covers adding a server to your client.

---

*Sources: [Smithery shutdown notice](https://www.smithery.ai/future), [Arcade: Smithery Is Now Part of Arcade.dev](https://arcade.dev/blog/smithery-joins-arcade) (August 5, 2026). Endpoint tests run by mcpinstall on November 8, 2026.*
