---
title: Remote MCP Servers: We Tested 1,500, 89% Still Work
date: 2026-11-18T17:29:10.409Z
---

**We sent a standard MCP handshake to 1,500 remote MCP servers picked at random from the official MCP Registry (1,489 had a testable URL). 89% of them answered: 56% completed the handshake with no credentials at all, and 32% replied that you need to sign in first. The other 11% were dead: 6.8% returned 404, 1.8% returned a server error, and 2.3% could not be reached.**

Almost half of the dead ones come from one place. A single publisher accounts for 2,365 Registry entries that all point at the same host, and that host returned 404 for every one we tried. Leave that host out and 94% of remote MCP servers are online. Another 216 entries still point at Smithery's hosted servers, and 2 of the 3 we sampled returned 404.

This article covers what we measured, why remote servers break, ten public servers you can test with no key, and a one-line check to run before you add any remote MCP server to Claude, Cursor or VS Code.

## How many remote MCP servers are there?

**The official MCP Registry lists 66,904 servers, and 45,552 of them (68%) offer a remote endpoint you can connect to by URL.** We crawled the whole Registry (`registry.modelcontextprotocol.io`, latest version of each server, 670 pages of 100) on November 18, 2026, between 17:14 and 17:18 UTC.

| | Count |
|---|---|
| Servers in the official MCP Registry | 66,904 |
| Servers with a remote endpoint | 45,552 |
| Servers with an installable package (npm, PyPI, OCI, MCPB…) | 25,507 |
| Distinct hostnames behind the remote endpoints | 34,434 |
| Change since our last crawl (63,501 on November 13) | +3,403 in 5 days |

Remote endpoints are concentrated more than the count suggests. The ten largest registered domains account for 10,916 of the 45,552 remote servers, or 24%. Some are hosting platforms where many developers run their own servers (`workers.dev` 3,742, `apify.com` 1,260, `vercel.app` 461, `onrender.com` 344, `railway.app` 317); others are single publishers that list hundreds or thousands of generated servers (`pipeworx.io` 1,936, `usefulapi.io` 1,060). For the full count of MCP servers across directories, see [how many MCP servers exist](https://www.mcpinstall.com/blog/how-many-mcp-servers).

## How many remote MCP servers actually work?

**89.1% of the remote MCP servers we probed were online, and 55.9% would talk to an anonymous client.** We picked 1,500 servers with a remote endpoint at random (fixed seed, so the sample can be reproduced), skipped 11 whose URL contains a placeholder like `{tenant}`, and sent each of the remaining 1,489 one MCP `initialize` request, the first message every client sends. No tools were called and no credentials were sent. Anything that failed was retried once a few minutes later.

| Result | Servers | Share | What it means for you |
|---|---|---|---|
| Handshake completed, no credentials needed | 832 | 55.9% | Works as soon as you add the URL |
| 401 or 403: sign-in required | 480 | 32.2% | Online. Your client will ask for OAuth or an API key |
| Answered, but not with a usable handshake | 15 | 1.0% | Online, but likely misconfigured or on an older transport |
| 404 or 410: not found | 101 | 6.8% | The endpoint is gone. Usually a moved or deleted server |
| 5xx server error | 27 | 1.8% | Up but broken: 503 (10), 530 (6), 500 (5), 502 (5), 521 (1) |
| Unreachable | 34 | 2.3% | Connection reset (28), TLS error (5), timeout (1) |
| **Online in total** | **1,327** | **89.1%** | |

A sample of 1,500 from 45,552 puts the margin of error at about ±1.6 percentage points at 95% confidence.

Some other things the handshakes showed:

- **Speed.** Among servers that completed the handshake, the median response took 628 ms and the 90th percentile 1,545 ms, measured from a single client in Asia.
- **Protocol versions.** Of the servers that completed the handshake and reported a version, 759 answered with `2025-06-18`, 27 with `2024-11-05`, 18 with `2025-03-26` and 3 with `2025-11-25`.
- **Transport.** 1,470 of the 1,500 endpoints use Streamable HTTP. The 30 that still use the older SSE transport fared worse: 9 of the 30 were not found or could not be reached.
- **Sign-in is announced properly.** 438 of the 480 servers that refused us sent a `WWW-Authenticate` header, which is how a client learns where to start OAuth.

## Why do remote MCP servers stop working?

**Mostly because a whole batch of entries points at one host that went away, or because the entry was published months ago and nobody updated it.** The Registry stores what the publisher submitted; it does not check that the URL still answers.

**One host, 2,365 dead entries.** 74 of the 1,500 servers in our sample, and 2,365 entries in the full Registry, come from a single publisher whose server names all end in `-ok` and whose endpoints all sit on one Cloudflare Workers host. Every one we tested returned 404. That one host accounts for 74 of the 162 failures in our sample. Without it, 1,327 of 1,415 servers (93.8%) are online.

**Smithery's hosted URLs are still listed.** 216 Registry entries point at `server.smithery.ai`. We drew three of them: two returned 404 and one asked for sign-in. If you are still running a server from a Smithery URL, our guide to [moving off Smithery's hosted URLs](https://www.mcpinstall.com/blog/smithery-shutdown-official-mcp-urls) lists the official endpoints that replace the most popular ones.

**Older entries decay.** Leaving out the single dead host, the share of servers that are online falls the further back the entry was published:

| Published | Servers in sample | Online |
|---|---|---|
| November 1–18, 2026 | 508 | 99.4% |
| October 2026 | 424 | 95.5% |
| July–September 2026 | 364 | 90.9% |
| January–June 2026 | 111 | 74.8% |
| 2025 | 8 | 3 of 8 |

One in four remote servers published in the first half of 2026 no longer answers. The 28 failures from that period are spread over more than 20 different hosts, most of them the publishers' own domains, and they fail every way there is: connection resets (9), 404 (8), server errors (7) and TLS errors (4). Nothing in the Registry marks them as stale.

## Which remote MCP servers can you test for free?

**Eight well-known remote MCP servers ran a real tool call for us with no key on November 18, 2026: Microsoft Learn, AWS Knowledge, Cloudflare Docs, Hugging Face, DeepWiki, GitMCP, Astro Docs and Exa.** We sent each one `initialize`, `tools/list` and one ordinary `tools/call` (a documentation search, a repository lookup or a web search), without credentials. Two more listed their tools but did not run one without a key.

| Server | Remote URL | Tools | Tool call without a key | Tool we called (time) |
|---|---|---|---|---|
| Microsoft Learn | `https://learn.microsoft.com/api/mcp` | 3 | ✅ Worked | `microsoft_docs_search` (0.8 s) |
| AWS Knowledge | `https://knowledge-mcp.global.api.aws` | 5 | ✅ Worked | `aws___search_documentation` (1.7 s) |
| Cloudflare Docs | `https://docs.mcp.cloudflare.com/mcp` | 2 | ✅ Worked | `search_cloudflare_documentation` (4.0 s) |
| Astro Docs | `https://mcp.docs.astro.build/mcp` | 1 | ✅ Worked | `search_astro_docs` (1.3 s) |
| DeepWiki | `https://mcp.deepwiki.com/mcp` | 3 | ✅ Worked | `read_wiki_structure` (0.4 s) |
| GitMCP | `https://gitmcp.io/docs` | 5 | ✅ Worked | `match_common_libs_owner_repo_mapping` (10.2 s) |
| Hugging Face | `https://huggingface.co/mcp` | 4 | ✅ Worked | `hub_repo_search` (0.3 s) |
| Exa | `https://mcp.exa.ai/mcp` | 2 | ✅ Worked | `web_search_exa` (0.4 s) |
| Context7 | `https://mcp.context7.com/mcp` | 2 | ❌ Anonymous quota used up; asks for a free key | `resolve-library-id` |
| API Pick | `https://www.apipick.com/mcp` | 27 | ❌ Needs an API key | `web_search` |

A tool call that works once without a key is not a promise of free use. Context7's reply shows that anonymous use has a monthly quota, and other providers can add limits at any time. API Pick (our sister product) lists its tools to anyone so clients can show them, but every tool call needs a key. Check each provider's terms before you build on one.

These eight popular remote servers answered with 401 and need you to sign in through your client: GitHub (`https://api.githubcopilot.com/mcp/`), Notion, Linear, Atlassian, Sentry, Stripe, Vercel and Semgrep. That is normal for any server that acts on your own account.

## How do you check a remote MCP server before adding it?

**Send it the same `initialize` request a client would and read the status code: 200 means it works, 401 means it works but needs sign-in, 404 or a connection error means don't bother.** This takes one command:

```bash
curl -s -o /dev/null -w "%{http_code}\n" -X POST "https://mcp.deepwiki.com/mcp" \
  -H "content-type: application/json" \
  -H "accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"check","version":"1.0"}}}'
```

| Status | Verdict |
|---|---|
| 200 | Works. Add it |
| 401 / 403 | Works, needs sign-in. Your client will open an OAuth window or ask for a key |
| 404 / 410 | Gone. Look for the publisher's current URL |
| 405 / 406 | Probably online but expects a different transport or headers; check the server's docs |
| 5xx | Online but broken right now. Try again later |
| No response / TLS error | Down or misconfigured. Don't add it |

Drop `-o /dev/null -w …` to see the reply itself: a working server answers with its name, version and the protocol version it supports.

If a server passes this check and still fails in your client, the problem is usually on the client side: an outdated client that only speaks the older SSE transport, a proxy that strips the `Authorization` header, or a timeout set too low. Our guide on [how to use MCP](https://www.mcpinstall.com/blog/how-to-use-mcp) covers adding remote servers to Claude, Cursor and VS Code.

## What does a 401 from an MCP server mean?

**A 401 means the server is up and wants you to authenticate; it is not an error in the server.** Remote servers that read or write your own data (GitHub, Notion, Linear, Stripe) all start this way. A client that supports MCP authorization reads the `WWW-Authenticate` header, discovers the server's OAuth endpoints and opens a browser window for you to sign in. Clients without OAuth support need an API key passed as a header instead, usually `Authorization: Bearer <key>`.

In our sample, 480 of 1,489 servers (32.2%) answered this way, and 438 of them included the `WWW-Authenticate` header that clients rely on.

## Limits of this test

- **Reachable is not the same as useful.** We checked that each server answers a handshake. We did not call tools, so a server can be online and still return errors or empty results.
- **One moment, one location.** The probe ran once on November 18, 2026 from one client in Asia, with one retry. A server that was briefly down shows as dead; a server blocked in some regions may look different from elsewhere.
- **Only the first remote endpoint.** Servers that list several endpoints were tested on the first one only.
- **Sign-in servers were not tested further.** We did not log in, so we cannot say whether the 32.2% behind a 401 work once you do.
- **Security is a separate question.** A working remote server can still be one you shouldn't trust. For local servers, see [what 63,501 servers show about MCP supply-chain security](https://www.mcpinstall.com/blog/mcp-server-supply-chain-security).

## FAQ

### Are there free remote MCP servers?

Yes. On November 18, 2026, eight well-known remote servers ran a real tool call for us without any key: Microsoft Learn, AWS Knowledge, Cloudflare Docs, Astro Docs, DeepWiki, GitMCP, Hugging Face and Exa. Context7 connects without a key but told us its anonymous monthly quota was used up, and API Pick needs a key for every tool call.

### Is there a remote MCP server I can use to test my client?

DeepWiki (`https://mcp.deepwiki.com/mcp`) and Microsoft Learn (`https://learn.microsoft.com/api/mcp`) are good test targets: both are public, need no key, respond in under 1.2 seconds in our test and expose a handful of read-only tools. Use GitHub's server (`https://api.githubcopilot.com/mcp/`) when you want to test your client's OAuth flow.

### Are MCP servers dead?

No. The official Registry grew by 3,403 servers in the five days to November 18, 2026, and 99% of remote servers published in November answered our handshake. What does die is individual listings: one in four remote servers published in the first half of 2026 no longer responds.

### Why is my remote MCP server not connecting?

Run the curl check above first. If it returns 200 or 401, the server is fine and the cause is on your side: a client without Streamable HTTP or OAuth support, a proxy removing headers, or a short timeout. If it returns 404 or nothing at all, the server itself is gone or down.

### Does the official MCP Registry check that servers work?

No. The Registry records what each publisher submits, including the remote URL, and does not test it. That is why 2,365 entries from one publisher still point at a host that returns 404, and why 216 entries still point at Smithery's hosted URLs.

## Method and sources

- **Registry crawl.** `GET https://registry.modelcontextprotocol.io/v0/servers?version=latest&limit=100`, following `nextCursor` until the end: 670 pages, 66,904 servers, November 18, 2026, 17:14–17:18 UTC.
- **Sample.** 1,500 servers drawn at random (fixed seed 20261119) from the 45,552 with a remote endpoint; the first listed remote endpoint of each was tested; 11 templated URLs were skipped.
- **Probe.** One JSON-RPC `initialize` request per endpoint (protocol version `2025-06-18`), 12-second timeout, 24 parallel connections, a user agent that names MCPInstall, no credentials and no tool calls. SSE endpoints received a `GET` with `accept: text/event-stream`. Timeouts, connection errors and 5xx responses were retried once.
- **Known servers.** The ten public servers above were tested with `initialize`, `notifications/initialized`, `tools/list` and one `tools/call`, without credentials, on November 18, 2026. The eight sign-in servers were tested with `initialize` only.
- Specification: [MCP transports](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports) and [MCP authorization](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization).
