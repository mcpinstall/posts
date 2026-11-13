---
title: Agent Skills Security: What 4,936 Audits Show
date: 2026-11-12T16:12:08.449Z
---

**Agent skills are not safe by default: a skill runs with whatever access your agent has, and most skills have never been scanned. Of the 101,333 skills in the MCPInstall catalogue, 4,936 (4.9%) carry a security audit, and of those 55% pass, 39% get a warning and 6% fail. Before installing one, read its SKILL.md, look at which scanner raised the flag and why, and check three lines: `allowed-tools`, any `curl … | bash`, and any mention of credentials.**

The numbers below come from the MCPInstall skills catalogue, which syncs from the skills.sh registry every day. I queried it on 2026-11-12. The audits attached to each skill come from five third-party scanners (Gen Agent Trust Hub, Socket, Snyk, Runlayer and ZeroLeaks) and were last refreshed on 2026-10-27. MCPInstall does not run these scanners; it shows their results on each skill page. I then opened four well-known skills that are marked Fail and read the files myself on 2026-11-13.

## How many agent skills have had a security audit?

**4,936 out of 101,333, or 4.9%.** The audited ones are the popular ones, so they cover most real usage.

| Measure | Count | Share |
|---|---|---|
| Skills in the catalogue | 101,333 | 100% |
| Skills with at least one audit | 4,936 | 4.9% |
| Installs that went to an audited skill | 250,289,182 of 310,638,491 | 80.6% |
| Skills with 10,000+ installs | 2,613 | |
| …of which audited | 2,544 | 97.4% |
| Skills with 1,000+ installs and no audit | 8,809 | |

Install counts are self-reported by the registry and include repeat installs, so read them as popularity, not as a count of people.

The practical reading: if a skill has more than 10,000 installs it almost certainly has an audit you can read. If it is one of the 8,809 skills with a thousand or more installs and no audit, nobody has checked it for you.

## What do the audits say?

**Among audited skills, 2,715 pass (55.0%), 1,919 get a warning (38.9%) and 302 fail (6.1%).** The overall status is the worst result from any scanner, so one scanner is enough to turn a skill red.

| Scanner | Skills scanned | Pass | Warn | Fail | Audit dates |
|---|---|---|---|---|---|
| Gen Agent Trust Hub | 4,925 | 4,645 | 137 | 143 | Feb to Oct 2026, median 2026-09-15 |
| Snyk | 4,926 | 3,635 | 1,229 | 62 | Feb to Oct 2026, median 2026-09-15 |
| Socket | 4,936 | 4,431 | 488 | 17 | Mar to Oct 2026, median 2026-09-15 |
| Runlayer | 1,811 | 1,046 | 651 | 114 | 2026-02-21 to 2026-03-16 only |
| ZeroLeaks | 2,279 | 2,220 | 59 | 0 | 2026-04-15 and 2026-04-16 only |

Two things stand out.

**The scanners rarely agree on a failure.** Of the 302 failed skills, 291 (96.4%) were passed by at least one other scanner, and only 32 (10.6%) were failed by two or more. In 270 cases a single scanner caused the Fail: Trust Hub alone 119 times, Runlayer alone 89, Snyk alone 52, Socket alone 10. Across all audited skills, 2,196 (44.5%) have a mix of pass and warn-or-fail results.

**Some results are old.** Every Runlayer result in the catalogue is dated between 21 February and 16 March 2026, and every ZeroLeaks result is from 15 or 16 April 2026. A skill that has been rewritten since then is still showing a verdict on files that no longer exist. The agent-browser case below is an example.

One more surprise: skills marked official fail slightly more often than community ones. Of 782 audited official skills, 57 fail (7.3%) and 449 get a warning (57.4%). Of 4,154 community skills, 245 fail (5.9%) and 1,470 get a warning (35.4%). Official skills tend to be vendor CLIs and automation tools, which do more on your machine and therefore trip more rules. A badge tells you who published a skill, not what it can do.

## What actually triggers a Fail? Four skills, read line by line

I picked four skills that are marked Fail and have very different reasons behind it, fetched today's files from their source and compared them with the scanner summaries.

| Skill | Installs | Who failed it | What I found in the file (2026-11-13) | My reading |
|---|---|---|---|---|
| [sentry-cli](https://www.mcpinstall.com/skills/site/cli.sentry.dev/sentry-cli) | 189,866 | Trust Hub (critical), Snyk (high) | Line 362 of SKILL.md v0.49.0: `curl https://cli.sentry.dev/install -fsS \| bash` | A real pattern, from the vendor's own domain. The same file offers `npm install -g sentry`; use that |
| [agent-browser](https://www.mcpinstall.com/skills/vercel-labs/agent-browser/agent-browser) | 1,488,560 | Runlayer: "9/11 files flagged", dated 2026-03-15 | Today the skill is one 3,316-byte SKILL.md with `allowed-tools: Bash(agent-browser:*)` | A stale verdict on files that are gone. The live risk is the one Trust Hub describes: it reads web pages, so a page can try to instruct your agent |
| [security-review](https://www.mcpinstall.com/skills/getsentry/skills/security-review) | 23,253 | Socket: "gptMalware"; Runlayer: 20/22 files | 22 reference files full of vulnerable code samples, such as `api_key = "sk-..."`, shown as things to flag | A false alarm: the "malware" is teaching material. The line I would look at is `allowed-tools: Read Grep Glob Bash Task`, which pre-approves unrestricted shell |
| [last30days](https://www.mcpinstall.com/skills/mvanhorn/last30days-skill/last30days) | 60,983 | Trust Hub (high): browser cookie databases, macOS Keychain, the `pass` manager; Runlayer: 304/604 files | SKILL.md v3.37.0 says it asks before reading browser cookies and does not read them unless configured or consented; `allowed-tools: Bash, Read, Write, AskUserQuestion, WebSearch` | The capability is real and the skill documents a consent step. Whether that is acceptable is your call, and you can only make it if you read the file |

Four Fail badges, four different meanings: a risky install method from a known vendor, an out-of-date scan, a false positive and a real sensitive capability with consent written in. The badge alone cannot tell you which one you are looking at. The scanner summary on the skill page usually can.

The same thing shows up in the summaries at scale. Of the 143 Trust Hub failures, 59 mention prompt injection, 30 mention credentials, tokens or secrets, 12 mention piping a remote script into a shell and 10 mention exfiltration. Socket's warnings and failures are mostly AI-classifier labels: `gptAnomaly` 375 times, `gptSecurity` 164, `gptMalware` 11 and `obfuscatedFile` 7.

## Which lines in a SKILL.md should you look for?

**Three lines tell you most of what a skill can do: the `allowed-tools` field, any command that pipes a download into a shell, and any reference to credentials.** I counted these across the 42,618 skills whose SKILL.md is stored in the catalogue.

| What to look for | Skills | Share of 42,618 | Why it matters |
|---|---|---|---|
| Only one file, the SKILL.md | 17,480 | 41.0% | Instructions only. Nothing runs unless your agent decides to run it |
| Ships script files (.py, .sh, .js, .mjs, .ts, .ps1 and similar) | 8,244 | 19.3% | Code the skill can ask your agent to execute. Read it |
| Declares `allowed-tools` | 4,697 | 11.0% | Tools the skill asks to have pre-approved |
| `allowed-tools` includes Bash | 3,073 | 7.2% | Shell access. 1,367 of these scope at least one Bash rule, like `Bash(git:*)`; the rest ask for all of it |
| `curl` or `wget` piped into a shell | 467 | 1.1% | Runs whatever the remote server returns at that moment |
| Names an API key, token or secret variable | 6,878 | 16.1% | Usually legitimate setup. Check where the value is sent |
| Mentions `~/.ssh` or `.aws/credentials` | 126 | 0.3% | Rarely needed. Ask why |
| Mentions `--dangerously-skip-permissions` or `bypassPermissions` | 73 | 0.2% | Asks you to turn the approval prompts off |

The download-into-shell line is the one that tracks the verdicts most closely. It appears in 10.8% of failed skills (32 of 296), 1.5% of warned skills (28 of 1,912) and 0.4% of passed skills (11 of 2,689). Shipping scripts is a weaker signal: 20.6% of failed skills ship scripts against 10.7% of passed ones. That matches a January 2026 academic study of 31,132 skills, which found that skills bundling executable scripts were 2.12 times more likely to contain a vulnerability than instruction-only skills ([Liu et al., arXiv 2601.10338](https://arxiv.org/abs/2601.10338)).

About `allowed-tools`: the [Agent Skills specification](https://agentskills.io/specification) defines it as a "space-separated string of tools that are pre-approved to run" and marks it experimental, with support varying by agent. In Claude Code the [skills documentation](https://code.claude.com/docs/en/skills) says the grant "goes through the normal permission flow". Either way it is the skill author telling you what the skill expects to be allowed to do, so it is the fastest line to read.

## How do you install a skill without trusting it blindly?

**Read it first, install it into one project, and pin what you installed.** The steps below use the open `skills` CLI ([vercel-labs/skills](https://github.com/vercel-labs/skills), which needs Node.js 22.20 or later) and Claude Code's own folders. I checked the commands against the CLI's README and the Claude Code docs on 2026-11-13. MCP servers have the same problem one layer down: a server pinned to an exact version can still pull new dependencies whenever its environment is rebuilt, as our [MCP server supply chain analysis](https://www.mcpinstall.com/blog/mcp-server-supply-chain-security) of 63,501 Registry servers shows.

| Step | What to do | Command or place |
|---|---|---|
| 1 | Read the audits and the SKILL.md before anything touches your disk | The skill's page in the [MCPInstall skills directory](https://www.mcpinstall.com/skills), section "Security audits" |
| 2 | Try it once without installing | `npx skills use owner/repo@skill-name` prints the skill as a prompt |
| 3 | Install into the current project, not your whole machine | `npx skills add owner/repo --skill skill-name -a claude-code` |
| 4 | Only go global once you trust it | add `-g` |
| 5 | Keep the lock file in git | `skills-lock.json` records a content hash per skill; `npx skills check` reports updates |
| 6 | Review updates like code changes | `npx skills update` changes what your agent is told to do |
| 7 | Remove what you no longer use | `npx skills remove skill-name`, or delete the folder |

Where the files end up in Claude Code, from its documentation: a personal skill lives at `~/.claude/skills/<skill-name>/SKILL.md` and loads in all your projects on that machine; a project skill lives at `.claude/skills/<skill-name>/SKILL.md` in the repository. Deleting the folder removes the skill, and Claude Code picks the change up in the running session.

Project-level install is the default in the CLI for a good reason. A skill in one repository can only be triggered while you work in that repository. A global skill is available to every session, including the one where your agent has production credentials loaded.

One detail worth knowing: the CLI's README says setting `DISABLE_TELEMETRY=1` or `DO_NOT_TRACK=1` disables "telemetry and security-audit requests". If you turn telemetry off, check the audits on the skill's page yourself.

If you are choosing among several skills for the same job, the [official skills list](https://www.mcpinstall.com/skills/official) and the [skill categories](https://www.mcpinstall.com/skills/categories) show the audit status next to each one, and the [trending Claude Code skills for November 2026](https://www.mcpinstall.com/blog/trending-claude-code-skills-november-2026) covers the audit notes for seven popular ones.

## Can you rely on a scanner alone?

**No. Use the scanners to decide what to read, not whether to trust.** Three independent results point the same way:

- In the catalogue data above, 96.4% of failed skills were passed by another scanner, and whole batches of results are seven to nine months old.
- CustomAI Studio ran NVIDIA's SkillSpector on the 61 skills in its own library and reported on 2026-11-09 that it flagged 22 lines in 13 skills, all harmless in context, while missing 11 skills that could act on connected apps, for example sending payment reminders to customers after one batch approval ([their write-up](https://customaistudio.io/articles/claude-skills-security-scan/)).
- The arXiv study found that 26.1% of skills contained at least one vulnerability and 5.2% showed high-severity patterns suggesting malicious intent, using its own detector. Different tools, different numbers.

Anthropic's own guidance is blunter than any scanner: "Use Skills only from trusted sources: those you created yourself or obtained from Anthropic", and if you must use one from an unknown source, "thoroughly audit it before use" ([Agent Skills overview, security considerations](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)). Its list of what to review is short: all bundled files, unexpected network calls, file access that does not match the stated purpose, and skills that fetch content from external URLs, because fetched content can carry instructions.

Scanners are good at patterns in code. They are weak on plain-language instructions such as "send the invoice reminder without asking", which is exactly what an instruction-only skill consists of, and 41% of skills are instruction-only.

## What this data cannot tell you

- **The audits are not mine.** They come from five third-party scanners through the skills.sh registry. I did not re-run any scanner, and I cannot see the detailed findings behind a summary such as "9/11 files flagged".
- **95.1% of skills have no audit at all.** Nothing here says those are safe or unsafe.
- **The pattern counts are text matches on SKILL.md only**, across the 42,618 skills whose content is stored. They do not look inside bundled scripts, and a match is not proof of a problem: a skill that documents `curl … | bash` as one install option is counted the same as one that tells the agent to run it.
- **I read four skills.** They illustrate why a Fail badge needs a second look; they are not a sample you can generalise from.
- **Audits lag behind the files.** The catalogue copies I compared were fetched on 2026-10-27; sentry-cli moved from version 0.46.0 to 0.49.0 and last30days from 3.30.0 to 3.37.0 by 2026-11-13.
- **A clean skill can change.** An update, or a URL the skill fetches, can alter its behaviour after you install it. That is what the lock file and reviewing updates are for.

## FAQ

### Are agent skills safe to install?

Some are and some are not, and the format gives you no guarantee either way. A skill is a set of instructions, sometimes with scripts, that your agent follows using its own access to your files, shell and connected apps. In the MCPInstall catalogue 4.9% of skills have a security audit; of those, 55.0% pass, 38.9% get a warning and 6.1% fail. Read the SKILL.md and the scanner notes before installing.

### How do I check a skill before installing it?

Open the skill's page and read the security audits, then read the SKILL.md itself. Look at the `allowed-tools` line, search for `curl` or `wget` piped into `bash` or `sh`, and search for words like token, cookie, keychain and `.ssh`. If the skill ships scripts, read those too. You can also run `npx skills use owner/repo@skill-name` to see the skill's content without installing it.

### What does a Fail audit on a skill mean?

It means at least one scanner rated the skill high risk. It does not mean the skill is malicious. In the catalogue, 96.4% of failed skills were passed by another scanner. The four I read failed for four different reasons: a download piped into a shell, an outdated scan, a false positive on teaching material and a real ability to read browser cookies with a consent step. Read the scanner's summary to see which case applies.

### How do I install agent skills in Claude Code?

Run `npx skills add owner/repo --skill skill-name -a claude-code` from your project. This installs into the project by default; add `-g` to install for all projects. You can also copy a skill folder by hand to `.claude/skills/<skill-name>/` in a repository or `~/.claude/skills/<skill-name>/` for personal use. The CLI needs Node.js 22.20 or later.

### How do I remove a skill I no longer trust?

Run `npx skills remove skill-name`, or delete the skill's folder under `.claude/skills/` or `~/.claude/skills/`. Then check what the skill may have changed while it was installed: files it wrote, tools it installed globally with npm or pip, and any credentials it was given. Removing the folder does not undo those.

### Is a skill with no audit more dangerous than one with a warning?

Not necessarily, but you know less about it. A warning comes with a summary you can read and judge. No audit means no scanner has looked: that is the case for 95.1% of the catalogue, including 8,809 skills with at least 1,000 installs. Treat an unaudited skill as unreviewed code and read every file.

If you want to see the audits for a specific skill, search for it in the [MCPInstall agent skills directory](https://www.mcpinstall.com/skills).
