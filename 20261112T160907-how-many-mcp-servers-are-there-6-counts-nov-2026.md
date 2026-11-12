---
title: How Many MCP Servers Are There? 6 Counts (Nov 2026)
date: 2026-11-12T16:09:07.845Z
---

**There is no single count of MCP servers. On November 12, 2026 the official MCP Registry listed 62,695 servers, Glama listed 120,008, LobeHub 121,182, GitHub had 42,886 repositories tagged `mcp-server`, MCPInstall indexed 23,254 and mcp.so 20,289. The gap comes from what each directory counts as one server, not from anyone miscounting.**

We read every one of those numbers ourselves that day. For the official registry we paged through its public API and counted the entries, which also tells us how servers are deployed and who publishes them. For Glama and LobeHub we have a snapshot per day since November 8, so we can say how fast they grow.

## How many MCP servers are there in November 2026?

Between about 20,000 and 121,000, depending on the directory. The six counts below were all read on November 12, 2026.

| Source | Count | What one "server" means there | Where the number comes from |
|---|---|---|---|
| [LobeHub MCP marketplace](https://lobehub.com/mcp) | 121,182 | A marketplace listing | Page heading "Explore 121,182 MCP Servers" |
| [Glama registry](https://glama.ai/mcp/servers) | 120,008 | An open-source server project | Page title "120,008 in the Glama Registry" |
| [Official MCP Registry](https://registry.modelcontextprotocol.io/) | 62,695 | A published server name (latest version) | Our full crawl of the public API |
| [GitHub topic `mcp-server`](https://github.com/topics/mcp-server) | 42,886 | A repository whose owner added the topic | GitHub search API `topic:mcp-server` |
| [MCPInstall](/browse) | 23,254 | A unique GitHub repository | Our own catalogue database |
| [mcp.so](https://mcp.so/) | 20,289 | A directory listing | Homepage "MCP.so at a glance" |

Two more numbers help with the remote-versus-local question. Glama lists [44,902 remote connectors](https://glama.ai/mcp/connectors) separately from its open-source servers. mcp.so says 1,378 of its 20,289 servers are remote, hosted servers.

We left out two directories. PulseMCP blocks scripted requests, so we could not read a count from it that day. Smithery stopped hosting connections on October 31 and no longer shows a total; see our list of [official MCP URLs to switch to](/blog/smithery-shutdown-official-mcp-urls) if that affected you.

## What does the official MCP Registry contain?

62,695 server names, of which 61,712 are active and 983 are marked deprecated. We requested `https://registry.modelcontextprotocol.io/v0/servers?version=latest&limit=100` and followed the cursor to the end: 627 pages, between 15:57 and 16:05 UTC on November 12, 2026. Every name appeared once.

| Measure | Servers | Share |
|---|---|---|
| Listed (latest version of each name) | 62,695 | 100% |
| Status active | 61,712 | 98.4% |
| Status deprecated | 983 | 1.6% |
| Declares a remote endpoint | 42,309 | 67.5% |
| Declares an installable package | 24,371 | 38.9% |
| Declares both | 4,673 | 7.5% |
| Declares neither | 688 | 1.1% |
| Name under `io.github.*` | 35,236 | 56.2% |
| No source repository listed | 22,183 | 35.4% |

Remote endpoints are almost all streamable HTTP: 41,570 servers declare it and 1,111 declare SSE. Packages are mostly npm.

| Package type | Servers |
|---|---|
| npm | 16,504 |
| PyPI | 5,407 |
| OCI (container image) | 1,771 |
| MCPB (MCP bundle) | 1,696 |
| NuGet | 179 |
| Cargo | 131 |

A server can ship several package types, so these rows overlap. Of the packages, 24,074 servers declare stdio transport.

## How fast is the number of MCP servers growing?

By roughly 500 to 800 servers a day in each of the three large directories we track.

| Directory | Earlier count | Later count | Change | Per day |
|---|---|---|---|---|
| Official MCP Registry | 59,603 (Nov 8) | 62,695 (Nov 12) | +3,092 | about 650–770 |
| Glama | 117,443 (Nov 9) | 119,476 (Nov 12) | +2,033 | about 680 |
| LobeHub | 119,623 (Nov 9) | 121,182 (Nov 12) | +1,559 | about 520 |

The Glama and LobeHub rows are our own snapshots of their public pages. From November 9 they were taken at the same time each day (01:16 UTC+8), so that pair is exactly three days apart; the November 8 snapshot was taken at 13:46.

| Date (2026) | Glama | LobeHub |
|---|---|---|
| Nov 8 | 117,432 | 119,429 |
| Nov 9 | 117,443 | 119,623 |
| Nov 10 | 118,203 | 120,046 |
| Nov 11 | 118,894 | 120,468 |
| Nov 12 | 119,476 | 121,182 |

Glama's title had moved on to 120,008 when we re-read it at 16:00 UTC on November 12, which is the figure in the first table. The registry range reflects that we know the day of the earlier count but not the hour.

The registry's November 8 figure is not ours. It comes from [an independent count published on DEV](https://dev.to/fast_data_asap/i-counted-every-server-in-the-official-mcp-registry-59603-entries-and-october-alone-added-16282-1jlj) that used the same API and the same "latest version per name" rule, which is why we compare against it. That author also dated each server's first publication and found 16,282 new servers in October 2026 alone.

Our crawl shows the same recency from another side: 33,859 of the 62,695 servers (54.0%) had their current version published in October or November 2026. That is the date of the latest version, not of the first one, so it mixes new servers with updated ones.

## Why do MCP directories disagree?

Because they count different things. Five differences explain most of the gap.

1. **The unit.** The official registry counts published names. MCPInstall counts GitHub repositories. Glama and LobeHub count listings. One repository can publish several registry names, and one product can appear as a repository, a package and a hosted endpoint.
2. **Who adds the entry.** Registry entries are published by the server's author. Glama, LobeHub, mcp.so and MCPInstall also crawl and import, so they pick up projects whose authors never registered anywhere.
3. **Remote-only servers.** 22,183 registry entries (35.4%) list no source repository. A catalogue keyed on GitHub URLs, like ours, cannot hold them.
4. **Duplicates and forks.** When we deduplicated our own catalogue we removed 1,305 duplicate rows and 135 entries that turned out not to be MCP servers. That was 1,440 of 24,694 rows, or 5.8%. A directory that skips this step reports a higher number for the same ecosystem.
5. **Bulk publishers.** The registry has 38,552 distinct publisher namespaces. 34,954 of them (90.7%) publish exactly one server, while the five largest publish 8,253 between them, 13.2% of the whole registry. The largest single namespace has 2,366 servers.

None of the six sources checks that a server still starts or answers. A listing is a claim that a server was published, not that it works today.

## Which number should you quote?

Pick the one that matches the claim you are making.

| If you want to say | Use | Why |
|---|---|---|
| How many servers authors have formally published | Official registry: 62,695 | Self-published, one row per name, public API anyone can re-run |
| How many projects exist in the wild | Glama or LobeHub: about 120,000 | Crawled, includes unregistered projects |
| How many open-source repositories self-identify as servers | GitHub topic: 42,886 | Owner-applied tag, undercounts untagged repositories |
| How many servers offer a hosted URL | Registry remote count: 42,309, or Glama connectors: 44,902 | Both count declared remote endpoints |

Always attach the date and the source. A count from September is already tens of thousands behind.

## What does MCPInstall's own catalogue look like?

Smaller and older than the big directories: 23,254 servers on November 12, 2026.

| Measure | Servers |
|---|---|
| Indexed | 23,254 |
| Keyed to a GitHub repository URL | 23,219 (99.8%) |
| Assigned to a category | 13,383 (57.6%) |
| Not yet categorised | 9,871 (42.4%) |
| Has a separate website link | 785 (3.4%) |
| Added in October–November 2026 | 854 |

Most of the catalogue was imported between April and June 2025. The three largest categories are Developer Tools (2,444), AI Chatbot (1,536) and API Development (1,081). You can [browse MCP servers](/browse) by category, or start with our shorter [MCP server list](/blog/mcp-server-list) if you only want the well-known ones.

## What these numbers do not tell you

- **Not usage.** No source here publishes install or call counts for servers. A count of listings says nothing about how many are used.
- **Not liveness.** We did not test whether any of the 62,695 registry servers respond. Neither does any directory in the table.
- **Not a union.** We did not match entries across directories, so we cannot say how many distinct servers exist in total. Adding the rows together would count many servers several times.
- **One day.** Every count is from November 12, 2026. The growth rates come from three to five days of data and should not be extrapolated over months.
- **Our catalogue undercounts remote servers.** MCPInstall does not yet store a remote endpoint URL per server, so we cannot report our own remote share.
- **GitHub's figure is a search estimate.** The topic count comes from the search API's `total_count`, and only includes repositories whose owners added the topic.

## FAQ

### How many MCP servers are there?

On November 12, 2026 the official MCP Registry listed 62,695 servers. Aggregators that also crawl unregistered projects list about 120,000: Glama showed 120,008 and LobeHub 121,182.

### How many MCP servers are in the official registry?

62,695 distinct server names, counted by paging through the registry's public API on November 12, 2026. 61,712 were active and 983 deprecated.

### How many MCP servers are remote?

In the official registry, 42,309 of 62,695 servers (67.5%) declare a remote endpoint, and 41,570 of those use streamable HTTP. Glama lists 44,902 remote connectors.

### Is there an official list of MCP servers?

Yes. The official MCP Registry at registry.modelcontextprotocol.io is run by the MCP project and has a public API that needs no key. Authors publish their own entries, so it does not include every server that exists.

### Why does Glama list twice as many servers as the official registry?

Glama indexes open-source projects it finds, whether or not the author published them to the registry. The registry only holds entries an author chose to publish. Glama's count is also per project, while the registry's is per published name.

### How do I count the registry myself?

Request `https://registry.modelcontextprotocol.io/v0/servers?version=latest&limit=100`, read `metadata.nextCursor` from the response, and repeat with `&cursor=<value>` until no cursor comes back. Our run took 627 requests and about eight minutes.

If you are new to the protocol, our guide on [how to use MCP](/how-to-use-mcp) covers installing a first server. If you work in Claude Code, the [trending Claude Code skills](/blog/trending-claude-code-skills-november-2026) list shows what people are installing alongside their servers this month.
