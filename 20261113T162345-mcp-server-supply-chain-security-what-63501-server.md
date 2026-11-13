---
title: MCP Server Supply Chain Security: What 63,501 Servers Show
date: 2026-11-13T16:23:45.696Z
---

MCP server supply-chain risk is the chance that code you never chose ends up running with your credentials, because a local MCP server's dependencies can change while its own version stays pinned. We measured how big that gap is in the official MCP Registry on November 13, 2026: of 63,501 servers, 19,951 run only as local packages, almost every npm and PyPI entry pins an exact top-level version, yet in a random sample only 1 of 494 npm packages ships the one lockfile `npx` honors, and only 5 of 462 PyPI packages pin every direct dependency. About one server in four (16,725) asks for an API key, token or other secret.

Two reports this week made the risk concrete. Corelight showed a "pinned" MCP server running 15 different dependency sets in five weeks, and AWS fixed a server-side request forgery flaw in the open-source mcp-gateway-registry. This article covers both, the Registry numbers behind them, and what to change in your own setup.

## What happened this week?

**Corelight: a pinned server that kept changing (published November 10, 2026).** Corelight investigated an administrator's workstation where AI coding assistants ran MCP servers holding credentials for the firewall, the SIEM and other management systems. Nothing was compromised. But one server, configured to run one release built from one commit, was rebuilt 19 times over five weeks and produced 15 distinct environments: 23 of its 69 packages changed version, including the cryptography library, the JWT library and the MCP SDK itself. Nobody had touched the configuration. The launcher (`uvx`) installed the package's declared dependency ranges from PyPI rather than the project's hash-pinned lock file. Of 35 MCP servers Corelight reviewed, 23 had a recommended install that resolves dependencies fresh from a public registry. Corelight stresses this is not a flaw in MCP; it is a consequence of how the servers are launched.

**AWS: SSRF in mcp-gateway-registry (bulletin published November 11, 2026).** CVE-2026-125926 affects mcp-gateway-registry versions 1.0.3 through 1.34.0, an Apache-2.0 project that combines an MCP server registry with a gateway that proxies tool calls. An authenticated user with permission to register or edit a server could set an upstream address that made the gateway send requests into the deployment's internal network, and in some cases read the responses. Version 1.35.0 validates upstream addresses on registration and edit, but does not re-check entries stored before the upgrade. The fix in 1.25.0 for a separate issue does not cover this one. AWS reports no confirmed exploitation.

The two stories sit at opposite ends of the same problem. One is the code on your laptop changing underneath you. The other is the box you put in front of your MCP servers to control them becoming a path into your network.

## How do the 63,501 servers in the official Registry get installed?

We crawled every server in the official MCP Registry (`registry.modelcontextprotocol.io`, latest version of each, 636 pages) on November 13, 2026, between 16:07 and 16:14 UTC. A server can list remote endpoints, installable packages, or both.

| Install path | Servers | Pinned how | Frozen by construction? |
|---|---|---|---|
| Remote endpoint only | 38,150 | Vendor runs it | Not your dependency tree; you trust the vendor |
| npm package | 16,741 | Exact top-level version on 17,021 of 17,036 entries; 15 say `latest` | No: dependencies resolve at install |
| PyPI package | 5,438 | Exact top-level version on every entry | No: dependencies resolve at install |
| OCI (container) image | 1,783 | Version tag on 1,737 images, `:latest` on 42, digest on 17 | Only when pinned by digest (0.9%) |
| MCPB bundle | 1,714 | SHA-256 of the file on 2,321 of 2,321 bundles | Yes: one hashed file |
| NuGet / Cargo | 181 / 133 | Exact version | No (Cargo without `--locked`, NuGet ranges) |

Servers with both a remote and a package count in both rows (4,710 of them), and 690 entries list neither.

The npm and PyPI rows are the ones that matter for the Corelight pattern. Between them they cover 22,179 servers, and nearly every entry looks pinned. A pinned top-level version is not a pinned environment.

## Why does a pinned MCP server still change?

Because the version you pin is only the first package in a tree.

- **npm.** `npx some-server@1.2.3` installs exactly 1.2.3, then resolves its dependencies within the ranges in its `package.json`. A project's `package-lock.json` is never published to the registry. The only lockfile that travels with a published package is `npm-shrinkwrap.json`, which npm's documentation calls "a publishable lockfile" and recommends for command-line tools.
- **PyPI with uvx.** `uvx` builds a disposable environment in the uv cache and reuses it, per the uv documentation, until a different version is requested, the cache is pruned or refreshed. When it does rebuild, it resolves the package's declared requirements again, not any lock file in the source repository. That is the rebuild cycle Corelight measured: a new environment every couple of days, started automatically by the assistant.

We sampled 500 npm and 500 PyPI servers from the Registry at random and read each pinned version's metadata from the npm registry and PyPI (494 of each resolved; the rest were unpublished or renamed):

| What we checked | npm (494) | PyPI (494) |
|---|---|---|
| Have runtime dependencies | 395 | 462 |
| Ship a lockfile the launcher honors | 1 (`npm-shrinkwrap.json`) | 0 (uvx ignores repository lock files) |
| Pin every direct dependency exactly | 23 | 5 |
| No upper bound on any dependency | not measured | 219 |
| Run an install script (`preinstall`, `install`, `postinstall`) | 15 | not applicable |
| Median direct dependencies | 2 | 3 |

Even the 23 npm packages that pin every direct dependency only lock the first layer. Their dependencies' dependencies still float. In practice, almost every local npm or PyPI MCP server in the Registry runs whatever versions the package registries serve on the day your environment is built.

## Why does it matter more for MCP servers than other packages?

Two properties, both from Corelight's write-up:

1. **Every dependency runs with the server's access.** The server loads its dependencies into its own process. Code in any of them can read the API tokens in its environment, the files the user can read and the networks the machine can reach. A malicious package does not have to be called; some recent payloads run when Python starts or the package loads.
2. **Nobody types the install command.** You add one line to a config file. From then on the assistant starts the server at the beginning of every session. Corelight counted 162 launches in one week for one server.

And the servers ask for valuable secrets. In the Registry, 10,087 servers request a secret through an environment variable and 7,292 through a remote header; 16,725 ask for at least one. Among the named variables: `ANTHROPIC_API_KEY` (157 servers), `OPENAI_API_KEY` (152), `GITHUB_TOKEN` (99), `DATABASE_URL` (57) and `EVM_PRIVATE_KEY` (45), a crypto wallet's private key.

Package hijacks are not hypothetical. Corelight lists xz Utils (2024), Ultralytics (2024), Nx (2025) and the Shai-Hulud worm, whose June 2026 variant reached PyPI, included MCP-themed packages and went after API keys for AI coding assistants. Malicious releases are often live for minutes or hours. A server that rebuilds every couple of days has more chances to land inside one of those windows.

## What does the mcp-gateway-registry flaw mean if you run a gateway?

A gateway or self-hosted registry exists so that a team can approve servers once and route every tool call through one controlled point. CVE-2026-125926 shows the flip side: the component that decides where requests go is itself an attack surface, and its network position is often more privileged than any single user's. If it runs with a cloud role attached, the role's credentials are in reach too.

What AWS recommends, in short:

- Upgrade to 1.35.0 or later, including any fork or derived build
- After upgrading, review every registered server's upstream address and remove any you did not intend; the fix does not re-validate stored entries
- Review application logs from the period an affected version ran for unexpected registrations or edits
- Until you can patch: limit who can register or edit servers, restrict outbound connections from the registry and auth server, require IMDSv2 on EC2 and trim the attached role's permissions

The general lesson applies to any MCP gateway: treat "who can register a server" as an admin permission, and give the gateway only the network reach it needs.

## How do you freeze an MCP server you depend on?

You do not need to stop using local servers. You need to make updates a decision instead of a side effect.

1. **Prefer an install format that is frozen by construction.** An MCPB bundle carries a SHA-256 hash in the Registry. A container image pinned by digest (`image@sha256:…`) cannot change underneath you; a tag can.
2. **Otherwise install once, from a lock file, into a dedicated environment.** Use the project's hash-pinned lock (`uv pip sync`, `pip install --require-hashes`, `npm ci` in a checkout) and launch the installed binary directly instead of `npx` or `uvx`. Corelight froze five servers this way; over 457 launches in two weeks, their package lists never changed, and startup dropped from 4–11 seconds to about 2.
3. **Turn off start-up update checks** where the server or its framework has a setting for it.
4. **Give each server its own narrow credential.** A read-only token scoped to one project limits what a bad dependency can take. Never hand a local server an administrator's key or a wallet private key unless that is the whole point of the server.
5. **Give updates an owner.** Someone reviews and bumps the lock on a schedule, ideally after a cooling-off period of a few days, so that short-lived malicious releases are caught by others first.
6. **Prefer the vendor's remote endpoint when there is one.** Then the dependency tree is the vendor's problem. After the Smithery shutdown we listed [official remote MCP URLs](https://www.mcpinstall.com/blog/smithery-shutdown-official-mcp-urls) for 20 popular servers. Beware the variant Corelight flagged: some vendors' instructions start a local auth proxy with `npx <proxy>@latest`, which pulls whatever was published most recently on every start.

## When is this not worth the effort?

- **Throwaway experiments with no credentials.** If the server only reads public data and holds no token, the blast radius is your user account, which is the same as running any other package.
- **Remote-only servers.** If you only connect to a vendor-hosted endpoint, you have nothing local to freeze; your risk is the vendor and the token you give it.
- **Servers you are developing yourself.** You already control the lock file; just launch from your checkout.

The case that most needs freezing is the combination Corelight found: a server that starts automatically, rebuilds on its own, and holds credentials to something important.

## How we measured

- **Registry crawl:** `GET https://registry.modelcontextprotocol.io/v0/servers?version=latest&limit=100`, 636 pages, November 13, 2026, 16:07–16:14 UTC. 63,501 servers: 62,486 active, 1,015 deprecated. Up from 62,695 a day earlier ([how many MCP servers are there](https://www.mcpinstall.com/blog/how-many-mcp-servers)).
- **Secrets:** a variable or header counts when the Registry marks it `isSecret` or its name looks like a key, token, password or credential.
- **Dependency sample:** 500 npm and 500 PyPI entries chosen at random (fixed seed) from entries with an exact version; metadata from `registry.npmjs.org` and `pypi.org` for that exact version; optional extras excluded on PyPI.
- **Limits:** the Registry records the package, not the command each client runs, so "launched with npx or uvx" is inferred from the package type (`runtimeHint` is filled in for only 3,174 npm and 2,499 PyPI entries). We did not run the servers. A server can also be installed in a frozen way by a careful user regardless of what it publishes.

## FAQ

### What is an MCP supply-chain attack?

It is a compromise delivered through something your MCP server already installs, such as a hijacked dependency release, rather than an attack on your machine directly. Because the server runs with your tokens and starts automatically, the bad code gets your credentials without anyone running an install command.

### Is pinning the MCP server version enough?

No. Pinning `server@1.2.3` or `server==1.2.3` fixes the top-level package only. Its dependencies are resolved again whenever the environment is built. In our sample, 1 of 494 npm MCP servers ships an `npm-shrinkwrap.json` and 5 of 462 PyPI servers pin every direct dependency.

### Are remote MCP servers safer than local ones?

For dependency drift, yes: there is no local dependency tree to drift. You are trusting the vendor's build process instead, plus whatever token you give the endpoint. 42,860 Registry servers list a remote endpoint.

### Which install formats are frozen?

MCPB bundles, which the Registry records with a SHA-256 hash (all 2,321 of them do), and container images pinned by digest (17 of 1,806 OCI entries). A Python or npm server becomes frozen when you install it from a hash-pinned lock file and launch it directly.

### Does CVE-2026-125926 affect MCP clients like Claude Code?

No. It affects deployments of the open-source mcp-gateway-registry project, versions 1.0.3 through 1.34.0. If you do not run that gateway or a fork of it, you are not affected by this CVE.

### How does this relate to agent skills?

Skills are mostly instructions, but some ship scripts that your agent runs. We covered what [4,936 agent skill audits](https://www.mcpinstall.com/blog/agent-skills-security) found separately; the same rule applies: know what runs, and who can change it.

If you want to see what a server ships before installing it, browse the [MCP server directory](https://www.mcpinstall.com/browse) or the [security testing servers](https://www.mcpinstall.com/mcp-server/categories/security-testing), and read [how to use MCP](https://www.mcpinstall.com/how-to-use-mcp) for the basics of configuring a client.

## Sources

Checked November 13, 2026.

- Corelight: [The supply-chain risk hiding in your MCP servers](https://corelight.com/blog/mcp-server-supply-chain-risk) (November 10, 2026)
- AWS: [Security bulletin 2026-180-AWS, CVE-2026-125926](https://aws.amazon.com/security/security-bulletins/2026-180-aws/) (November 11, 2026)
- agentic-community: [mcp-gateway-registry release 1.35.0](https://github.com/agentic-community/mcp-gateway-registry/releases/tag/1.35.0)
- npm Docs: [npm-shrinkwrap.json](https://docs.npmjs.com/cli/v11/configuring-npm/npm-shrinkwrap-json)
- uv Docs: [Tools](https://docs.astral.sh/uv/concepts/tools/)
- Official MCP Registry: [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io/)
- Coverage: Cyber Security News on [the Corelight research](https://cybersecuritynews.com/hackers-could-exploit-mcp-server) and [the AWS fix](https://cybersecuritynews.com/aws-mcp-gateway-security-flaw)
