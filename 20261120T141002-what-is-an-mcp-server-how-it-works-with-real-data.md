---
title: What Is an MCP Server? How It Works, With Real Data
date: 2026-11-20T14:10:02.843Z
---

**An MCP server is a small program that gives an AI app, such as Claude, ChatGPT, Cursor or VS Code, a set of tools, data and prompts it can use, all through one standard called the Model Context Protocol (MCP).** The AI app connects to the server, asks what it offers, and calls its tools when the model decides they help: read a file, query a database, open a GitHub issue, search the web. The server does the work and sends back the result. The model never touches your systems directly.

An MCP server can run on your own computer, started by the AI app as a background process, or on the internet at a URL. Either way it speaks the same protocol, so one server works with every app that supports MCP.

This guide explains what an MCP server is and how one works, shows a real exchange with a server we built and ran, and reports what we measured on November 20, 2026: of 816 public MCP servers that answer without a key, 31% already support the newest version of the protocol.

| MCP at a glance | As of November 20, 2026 |
|---|---|
| Current protocol version | `2026-07-28` (the previous one was `2025-11-25`) |
| Message format | JSON-RPC 2.0 |
| Ways to connect | stdio (local process) or Streamable HTTP (a URL) |
| What a server can offer | Tools, resources and prompts |
| Servers in the official MCP Registry | 66,904 (our crawl, November 18, 2026) |
| Servers listed on MCPInstall | 23,621 |
| Public servers supporting `2026-07-28` | 31.0% of 816 we tested |

## What is an MCP server, in plain terms?

**An MCP server is an adapter between an AI model and one system, such as your files, GitHub, a database or a web API, that describes what the model can do there and carries out the requests.** Before MCP, every AI app had to build its own integration for every service. With MCP, a service is wrapped once as a server, and any MCP-compatible app can use it.

Three roles take part in every connection:

| Role | What it is | Example |
|---|---|---|
| MCP host | The AI application you use | Claude Code, Claude Desktop, VS Code, Cursor |
| MCP client | The connector inside the host, one per server | The link VS Code opens to the Sentry server |
| MCP server | The program that provides tools and data | The GitHub server, the filesystem server, a database server |

The [official architecture overview](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture) puts it this way: the host creates one MCP client for each MCP server it connects to, and each client keeps a dedicated connection to its server. When people say "an MCP" or "an MCP tool", they almost always mean an MCP server.

The protocol was inspired by the Language Server Protocol, which lets any code editor support any programming language. MCP does the same for AI apps and the tools they can use.

## How does an MCP server work?

**The AI app asks the server what it offers, shows the tool descriptions to the model, and runs a tool when the model asks for it.** In the `2026-07-28` version of the protocol, there is no handshake and no session: every request carries the protocol version it uses, and each one stands on its own.

1. **Connect.** For a local server, the host starts it as a subprocess and talks to it over standard input and output (stdio). For a remote server, the host sends HTTP POST requests to its URL.
2. **Discover.** The client sends `server/discover`. The server replies with the protocol versions it supports and what it offers: tools, resources, prompts.
3. **List tools.** The client sends `tools/list`. Each tool comes back with a name, a description and a JSON Schema for its inputs.
4. **The model decides.** The host passes the tool descriptions to the model with your message. If the model wants a tool, it says which one and with what arguments.
5. **Call.** The client sends `tools/call`. The server runs the tool and returns the result, which goes back to the model.

Here is that exchange with a one-tool server we wrote with the official Python SDK (version 2.3.0) and ran on November 20, 2026. The messages are copied from the terminal and shortened for readability: `…` marks removed parts, and a few fields such as the cache hints and the output schema are left out.

```json
→ {"jsonrpc":"2.0","id":1,"method":"server/discover","params":{"_meta":{"io.modelcontextprotocol/protocolVersion":"2026-07-28","io.modelcontextprotocol/clientCapabilities":{}}}}
← {"jsonrpc":"2.0","id":1,"result":{"supportedVersions":["2026-07-28"],"capabilities":{"tools":{"listChanged":true},"resources":{…},"prompts":{…}},"resultType":"complete","_meta":{"io.modelcontextprotocol/serverInfo":{"name":"world-clock","version":""}}}}

→ {"jsonrpc":"2.0","id":2,"method":"tools/list","params":{"_meta":{…}}}
← {"jsonrpc":"2.0","id":2,"result":{"tools":[{"name":"time_in","description":"Current local time in an IANA time zone, e.g. Europe/Berlin.","inputSchema":{"type":"object","properties":{"city_timezone":{"title":"City Timezone","type":"string"}},"required":["city_timezone"]},"outputSchema":{…}}],"resultType":"complete",…}}

→ {"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"time_in","arguments":{"city_timezone":"Europe/Berlin"},"_meta":{…}}}
← {"jsonrpc":"2.0","id":3,"result":{"content":[{"type":"text","text":"2026-11-20 15:05 CET"}],"isError":false,"resultType":"complete"}}
```

The model only ever sees the tool's name, description and input schema, then the text that comes back. The code that runs, and any credentials it needs, stay on the server's side.

## What can an MCP server provide?

**A server can offer three kinds of things: tools the model can run, resources the app can read, and prompts the user can pick.** Most servers in practice offer only tools.

| Primitive | What it is | Who uses it | Example |
|---|---|---|---|
| Tools | Functions with typed inputs | The model decides when to call them | `create_issue`, `run_query`, `search_web` |
| Resources | Read-only data with a URI | The app, or the user, attaches them as context | A file, a database schema, a log |
| Prompts | Reusable message templates | The user picks them, often as a slash command | "Review this pull request" |

A server can also ask the user for something mid-task through the client, a feature called elicitation, for example to confirm an action or fill in a missing value. Three older features, roots, sampling and protocol-level logging, were deprecated in `2026-07-28`: they still work but new servers should not rely on them. Optional extensions add more on top, such as Tasks for long-running jobs and MCP Apps for interactive UI inside the chat.

## What is the difference between a local and a remote MCP server?

**A local server runs on your machine as a process the AI app starts; a remote server runs on someone's infrastructure and you connect to it by URL.** The protocol messages are the same; the transport and the trust model differ.

| | Local (stdio) | Remote (Streamable HTTP) |
|---|---|---|
| How you add it | A command, e.g. `npx …` or `uv run …` | A URL, e.g. `https://mcp.example.com/mcp` |
| Where the code runs | Your computer | The provider's servers |
| Typical clients | One: the app that started it | Many users at once |
| Sign-in | Usually an API key in an environment variable | Usually OAuth in the browser |
| Good for | Files, local databases, your shell, dev tools | SaaS products: GitHub, Notion, Sentry, Stripe |
| Main risk | It can do anything your user account can do | Your data goes to a third party; the URL can disappear |

Remote servers have grown fastest. In the official Registry, 45,552 of 66,904 servers (68%) offer a remote endpoint. In our test of 1,500 of them, 89% were online and 11% were dead. The full results are in [Remote MCP servers: we tested 1,500](https://www.mcpinstall.com/blog/remote-mcp-servers).

## What changed in MCP in 2026?

**The `2026-07-28` revision made MCP stateless: the `initialize` handshake and protocol sessions are gone, and every request carries its own version and capabilities.** That makes remote servers much easier to run behind ordinary load balancers and serverless platforms, because any instance can answer any request. The main changes, from the [official changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog):

| Change in `2026-07-28` | What it means |
|---|---|
| No `initialize` handshake, no `Mcp-Session-Id` | Each request is self-contained; servers keep no per-connection state |
| New `server/discover` method, required for servers | One call returns supported versions, capabilities and server identity |
| `subscriptions/listen` replaces the GET stream and resource subscribe | One opt-in stream for change notifications |
| Multi round-trip requests | A server that needs input returns `input_required`; the client retries with the answer |
| Roots, sampling and logging deprecated | Pass paths as tool arguments; call LLM APIs directly; log to stderr or OpenTelemetry |
| HTTP+SSE transport formally deprecated | Use Streamable HTTP |
| Dynamic Client Registration deprecated for OAuth | Client ID Metadata Documents replace it |

Older clients still work with most servers. The official Python SDK 2.3.0 server we built answered both the new `server/discover` and an old-style `initialize` request for protocol `2025-06-18`. Note for Python developers: in SDK 2.x, `FastMCP` was renamed `MCPServer` and now lives in `mcp.server.mcpserver`. The old import fails with a message that points to the [migration guide](https://py.sdk.modelcontextprotocol.io/v2/migration/).

## How many MCP servers support the 2026-07-28 protocol?

**31.0% of the public MCP servers we tested answered a `2026-07-28` request; 57.4% still speak only the older protocol.** We took the 832 remote servers that completed a standard handshake without credentials in our [November 18 Registry test](https://www.mcpinstall.com/blog/remote-mcp-servers). On November 20, 2026, between 14:04 and 14:05 UTC, we sent each of the 816 Streamable HTTP endpoints one `server/discover` request with `MCP-Protocol-Version: 2026-07-28`. No tools were called and no credentials were sent.

| Result | Servers | Share |
|---|---|---|
| Supports `2026-07-28` (listed in `supportedVersions`) | 253 | 31.0% |
| Older protocol only: "method not found" or "server not initialized" | 468 | 57.4% |
| Asked for credentials on this request | 64 | 7.8% |
| Answered `server/discover` but listed only older versions | 5 | 0.6% |
| Other errors or no answer | 26 | 3.2% |
| **Total** | **816** | |

Two details matter when reading this:

- **One publisher inflates the number.** All 70 servers in the sample from `pipeworx.io` support the new version. Without them, 183 of 746 servers (24.5%) do.
- **Supporting the new version rarely means dropping the old one.** 129 of the 253 also list one or more older versions in `supportedVersions`, and all 253 had answered an old-style `initialize` request two days earlier. The median `server/discover` response took 708 ms from our single client in Asia.

For you as a user, this means version mismatches are not yet a practical problem: current clients still speak the older versions, and most servers still accept them. For server authors, moving to an SDK release that supports `2026-07-28` gets you the stateless transport while keeping old clients working.

## How do you build a simple MCP server?

**With an official SDK, a working MCP server is about ten lines: create a server, decorate a function as a tool, and run it.** The SDK turns the function's signature and docstring into the tool's input schema and description. This is the server from the exchange above:

```python
from datetime import datetime
from zoneinfo import ZoneInfo

from mcp.server.mcpserver import MCPServer

mcp = MCPServer("world-clock")


@mcp.tool()
def time_in(city_timezone: str) -> str:
    """Current local time in an IANA time zone, e.g. Europe/Berlin."""
    return datetime.now(ZoneInfo(city_timezone)).strftime("%Y-%m-%d %H:%M %Z")


if __name__ == "__main__":
    mcp.run()  # stdio: the host app starts this file as a subprocess
```

Save it as `server.py` and add it to Claude Code (Python 3.10 or later, with `uv` installed):

```bash
claude mcp add --transport stdio world-clock -- uv run --with mcp==2.3.0 python /absolute/path/to/server.py
```

Then ask Claude "what time is it in Tokyo?" and it will call `time_in`. On our run, the same call returned `2026-11-20 23:06 JST`. Official SDKs also exist for TypeScript (`@modelcontextprotocol/sdk`, version 1.32.2 on November 20, 2026) and several other languages; the [SDK list](https://modelcontextprotocol.io/docs/2026-07-28/sdk) has them all.

## How do you add an existing MCP server to Claude, Cursor or VS Code?

**For a remote server, paste its URL into the app's MCP settings or run one command; for a local server, give the app the command that starts it.** In Claude Code, per [Anthropic's MCP documentation](https://code.claude.com/docs/en/mcp):

```bash
# remote server, by URL
claude mcp add --transport http notion https://mcp.notion.com/mcp

# local server, by command (everything after -- is the command)
claude mcp add --env AIRTABLE_API_KEY=YOUR_KEY --transport stdio airtable -- npx -y airtable-mcp-server
```

Cursor, VS Code, Windsurf, ChatGPT and Claude Desktop each have their own settings screen or JSON file for the same two pieces of information. Our [how to use MCP guide](https://www.mcpinstall.com/how-to-use-mcp) walks through each client, and you can [browse MCP servers by category](https://www.mcpinstall.com/browse) to find one with install instructions.

## Are MCP servers safe to use?

**An MCP server is code you let an AI model drive, so treat it like any program you install: it is as safe as its author and the permissions you give it.** The specification says tools "represent arbitrary code execution" and that tool descriptions should be treated as untrusted unless they come from a trusted server. The risks are concrete:

- **A local server runs as you.** It can read anything your user account can read and run any command it is written to run.
- **Tool descriptions are read by the model.** A malicious server can hide instructions in a description, which is known as tool poisoning.
- **Packages change.** An `npx` or `uvx` command without a pinned version fetches whatever was published last.
- **Remote servers see your data.** Everything the model sends to a tool goes to the server's operator.

Before adding a server, check that it comes from the vendor or a known maintainer, pin the version, give it the narrowest API key that works, and keep your client's tool-approval prompts on. Our [MCP server supply chain guide](https://www.mcpinstall.com/blog/mcp-server-supply-chain-security) covers the checks in detail.

## When do you not need an MCP server?

**If the model only needs a command-line tool it can already run, or a single fixed API call inside your own code, an MCP server adds a layer you don't need.** Coding agents like Claude Code and Codex can already run `git`, `gh` or `psql` in a terminal, and for many developer tasks that is enough. An MCP server pays off when you want the same capability in several AI apps, when the app has no shell (ChatGPT, Claude on the web), or when the service needs OAuth sign-in rather than a key on disk. For instructions and workflows rather than new capabilities, a [Claude skill](https://www.mcpinstall.com/skills) is often the lighter choice.

## Frequently asked questions

### What is an MCP server in AI?

It is the part of the Model Context Protocol that provides capabilities to an AI app. The AI app (the host) connects to MCP servers to get tools it can call, data it can read and prompt templates, so the model can act on real systems instead of only producing text.

### Is an MCP server the same as an API?

No. An MCP server usually wraps one or more APIs and presents them in a standard form a model can discover and call. An API is built for programmers who read its docs; an MCP server describes itself, so an AI app can learn what it does at runtime.

### Is an MCP server itself an AI?

No. Most MCP servers contain no model at all. They are ordinary programs that run a function when asked. The intelligence is in the AI app's model, which decides when to call them.

### Do I need to host an MCP server myself?

Not for remote servers: the vendor hosts them and you connect by URL. Local servers run on your computer and the AI app starts and stops them for you, so there is nothing to host either. You only host a server yourself if you build one for others to use.

### Are MCP servers free?

The protocol and the official SDKs are free and open source, and most servers on GitHub are free to run. The services behind them may charge: a server for a paid API still needs that API's key and plan.

### What does "MCP" stand for?

Model Context Protocol. Anthropic introduced it in November 2024 as an open standard, and it is now supported by apps from Anthropic, OpenAI, Google, Microsoft and others.

### How many MCP servers are there?

The official MCP Registry listed 66,904 servers on November 18, 2026, and MCPInstall lists 23,621. Counts across all directories, and how many are duplicates, are in [how many MCP servers exist](https://www.mcpinstall.com/blog/how-many-mcp-servers).

## Sources

Checked on November 20, 2026.

- Model Context Protocol: [specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28), [changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog), [server/discover](https://modelcontextprotocol.io/specification/2026-07-28/server/discover), [architecture overview](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture)
- MCP Python SDK 2.3.0 on [PyPI](https://pypi.org/project/mcp/) and its [v2 migration guide](https://py.sdk.modelcontextprotocol.io/v2/migration/)
- Anthropic: [Connect Claude Code to tools via MCP](https://code.claude.com/docs/en/mcp)
- MCPInstall measurements: `server/discover` probe of 816 servers on November 20, 2026; Registry crawl and handshake test of 1,500 servers on November 18, 2026
