---
title: Trending Claude Code Skills (November 2026): What They Do
date: 2026-11-08T06:53:50.241Z
---

![Neon skill cards for design, knowledge graphs, memory and animation fanning out of a command-line terminal under a rising trend arrow, representing trending Claude Code skills in November 2026](https://www.mcpinstall.com/api/r2-image?fullpath=blog%2Ftrending-claude-code-skills-november-2026-hero.jpg)

**The Claude Code skills people are searching for in November 2026 are ponytail (write less code), taste-skill, Uizze's UI Taste, impeccable and Emil Kowalski's skills (better-looking frontends), graphify (a knowledge graph of your codebase) and claude-mem (memory across sessions). Below is what each one does, how many installs it has, what its security audit says, and the one command to install it. Updated November 12: two more names broke out in search this week, the awesome-gamedev-agent-skills pack and OmniRoute, and both are covered in their own section below.**

Google Trends shows searches for these names rising fast over the past three months: "ponytail skill" up 550% to 900%, "graphify claude" up 250% to 1,650%, "claude mem" up 170% to 850%, and "taste skill", "impeccable skill" and "emil kowalski design skill" each up around 200%. Searches for "claude skills" as a whole rose 22% over the same window. The install counts below come from the skills.sh registry, as mirrored in the mcpinstall directory on November 8, 2026.

## The list at a glance

| Skill | What it does | Total installs | Last 24h | Audit | Page |
|---|---|---|---|---|---|
| Uizze UI Taste | Anti-"AI slop" UI playbooks for web and iOS | 1,046,083 | +29,448 | Not yet audited | [ui-taste](https://www.mcpinstall.com/skills/site/uizze.sh/ui-taste) |
| taste-skill (design-taste-frontend) | Art direction for landing pages, from 247 gallery sites | 747,036 | +6,117 | Warn | [design-taste-frontend](https://www.mcpinstall.com/skills/leonxlnx/taste-skill/design-taste-frontend) |
| Emil Kowalski: emil-design-eng | UI polish and animation judgement | 427,072 | +3,209 | Pass | [emil-design-eng](https://www.mcpinstall.com/skills/emilkowalski/skills/emil-design-eng) |
| impeccable | Design, critique and polish any frontend | 392,930 | +2,811 | Warn | [impeccable](https://www.mcpinstall.com/skills/pbakaus/impeccable/impeccable) |
| ponytail | "Lazy senior dev": the smallest change that solves the task | 122,321 | +1,611 | Pass | [ponytail](https://www.mcpinstall.com/skills/dietrichgebert/ponytail/ponytail) |
| graphify | Turns a codebase or folder into a queryable knowledge graph | 12,943 | +153 | Warn | [graphify](https://www.mcpinstall.com/skills/graphify-labs/graphify/graphify) |
| claude-mem (mem-search) | Searches your memory of past agent sessions | 9,259 | +20 | Warn | [mem-search](https://www.mcpinstall.com/skills/thedotmack/claude-mem/mem-search) |

"Audit" is the overall result shown on each skill's page, which combines up to five scanners (Gen Agent Trust Hub, Socket, Snyk, Runlayer, ZeroLeaks). **Pass** means no scanner raised a concern. **Warn** means at least one scanner flagged something worth reading before you install. It is not a verdict that the skill is malicious. Install counts are self-reported by the registry and include repeat installs.

## Four of the seven are about the same problem: AI-made UI looks generic

The single clearest pattern in this list is that developers are tired of interfaces that look like every other AI-generated page. Four skills attack that from different angles.

### Uizze UI Taste

**What it does:** gives your agent a workflow for building and reviewing web and iOS interfaces, with "anti-ui-slop" playbooks and optional references to real products. Its own description says it is designed for Claude Code, Codex, Cursor and GitHub Copilot.

**Good for:** teams that want one house style for UI work across several coding agents.

**Not for:** projects without a UI. It has also not been through mcpinstall's audit scanners yet, so read its SKILL.md first.

```bash
npx skills add https://uizze.sh --skill ui-taste
```

### taste-skill (design-taste-frontend)

**What it does:** art direction for public-facing web pages. Its SKILL.md says the directions are "distilled from a study of 247 curated gallery sites (One Page Love, Awwwards Sites of the Day)". It picks a direction (composition, type, palette, photography), enforces a no-slop house style and runs a look-and-fix loop on screenshots in a real browser.

**Good for:** landing pages, marketing sites, portfolios and product pages. It also covers dashboards and settings screens.

**Not for:** backend work. Its audit is Warn: Socket raised one "gptAnomaly" alert and Snyk rated it medium risk with one issue, while Gen Agent Trust Hub, Runlayer and ZeroLeaks passed it. Trust Hub also notes it can search for and download openly licensed media, so check where images come from in client work.

```bash
npx skills add https://github.com/leonxlnx/taste-skill --skill design-taste-frontend
```

### impeccable

**What it does:** a broad frontend design skill: design, redesign, critique, audit, polish, simplify, animate. It covers hierarchy, typography, color, motion, UX copy, accessibility and design systems (DESIGN.md, tokens), for web and native iOS or Android screens.

**Good for:** a second pass on an existing UI, or when you want a critique rather than new code.

**Not for:** non-UI tasks, as its description says. Its audit is Warn because Socket raised one "gptAnomaly" alert; Snyk, Gen Agent Trust Hub and ZeroLeaks passed it. Trust Hub notes it downloads a helper binary (`impeccable-engine`) from the author's GitHub for tasks like pixel diffing.

```bash
npx skills add https://github.com/pbakaus/impeccable --skill impeccable
```

### Emil Kowalski's skills (emil-design-eng)

**What it does:** encodes designer Emil Kowalski's approach to UI polish, component design and animation decisions. The same repository ships related skills, including `review-animations` (290,150 installs) and `apple-design` (280,514 installs).

**Good for:** motion and micro-interactions, where generic AI output is weakest. It is guidance only; the audit found no executable code.

**Not for:** layout or copy. Pair it with one of the skills above if you need a full redesign.

```bash
npx skills add https://github.com/emilkowalski/skills --skill emil-design-eng
```

## ponytail: make the agent write less code

**What it does:** ponytail tells the agent to behave like "a lazy senior developer": solve the whole problem with the least new code, reuse what already exists, and end each reply with what it skipped and any risk. It stays on for the session, with three levels (`/ponytail lite|full|ultra`), and turns off with "stop ponytail".

**Good for:** codebases where agents keep adding abstractions, helper files and dependencies you did not ask for. Its audit passed and notes it explicitly keeps security, validation and error handling.

**Not for:** greenfield prototypes where you want the agent to scaffold a lot quickly. The same author also publishes `ponytail-review` (66,683 installs) and `ponytail-audit` (54,540 installs) for reviewing a change or a whole repo.

```bash
npx skills add https://github.com/dietrichgebert/ponytail --skill ponytail
```

## graphify: a map of your codebase

**What it does:** turns a folder (code, docs, papers, even images and video) into a persistent knowledge graph with community detection, then answers questions about architecture and file relationships from it. It outputs an interactive HTML view, GraphRAG-ready JSON and a plain-language `GRAPH_REPORT.md`. Run `/graphify` in a repo, or point it at a GitHub URL.

**Good for:** onboarding to an unfamiliar codebase, or large repos where the agent keeps reading the wrong files.

**Not for:** small projects the agent can already read in full. Its audit is Warn because Socket raised one "gptAnomaly" alert; Snyk and Gen Agent Trust Hub passed it, and Trust Hub notes it includes secret detection so credentials are not indexed.

```bash
npx skills add https://github.com/graphify-labs/graphify --skill graphify
```

## claude-mem: memory across sessions

**What it does:** the `mem-search` skill searches your Claude-Mem memory on cmem.ai, which stores timestamped observations from past sessions across Claude Code, Codex and other agents. You ask "what did we do last session" and it searches that history.

**Good for:** long-running projects where you keep re-explaining decisions to a fresh session.

**Not for:** anyone who cannot send project context to a third-party service. Gen Agent Trust Hub notes it "transmits user search queries to this third-party service". Its overall Warn comes from Runlayer flagging its one file; Socket, Snyk and ZeroLeaks passed it. It needs a Claude-Mem account and API key.

```bash
npx skills add https://github.com/thedotmack/claude-mem --skill mem-search
```

## Added November 12: two new breakout searches

On November 12, 2026, Google Trends listed two new rising queries next to "agent skills" as Breakout, its label for growth too large to show as a percentage: "awesome gamedev agent skills" and "omniroute claude". The first is a skill pack. The second is not a skill at all, which is worth knowing before you search for one.

| Name | What it is | Skills in the mcpinstall directory | Total installs | Last 24h | Audit | Page |
|---|---|---|---|---|---|---|
| awesome-gamedev-agent-skills | 79 game-development skills plus a router, for 12 engines | 80 | 777,218 | +25,596 | 71 Pass, 9 not yet audited | [gamedev-skills](https://www.mcpinstall.com/skills/gamedev-skills/awesome-gamedev-agent-skills) |
| OmniRoute | An AI gateway; its repo also ships skills for operating it | 46 | 56,867 | +1,048 | Not yet audited | [omniroute](https://www.mcpinstall.com/skills/cheaper-inference/omniroute) |

Counts are from the mcpinstall directory on November 12, 2026. The totals add up every skill in the repository, and one full install of a pack counts once per skill, so they are not comparable with the single-skill numbers in the first table.

### awesome-gamedev-agent-skills

**What it does:** gives a coding agent 79 game-development skills and a `router` skill that picks among them. You describe the task; the router detects the engine and loads the two or three skills that fit. It covers Godot, Unity, Unreal, Ebitengine, Phaser, PixiJS, three.js, Bevy, pygame, LÖVE, Defold and Roblox, plus engine-independent topics such as game feel, level design, save systems and game AI. The [repository](https://github.com/gamedev-skills/awesome-gamedev-agent-skills) is Apache-2.0 and had 8,534 GitHub stars on November 12.

**Installs:** the most installed single skill is `game-feel` (15,507, +451 in 24 hours), and the `router` itself has 11,054 (+354). The pack first appeared in the directory on October 27.

**Good for:** building a game with an agent in an engine it handles poorly out of the box. The README says each skill is pinned to a current engine release, that code examples were run or compiled on 10 of the 12 engines, and that in 366 blind test prompts the right skill loaded 98.7% of the time. Those are the authors' own figures; we have not reproduced them.

**Not for:** agents that already carry many skills. The README itself advises installing only the router and your engine's bundle in that case, because skill descriptions get cut past the agent's limit and routing gets worse. On audits, 71 of the 80 skills show Pass and 9 have not been scanned yet. Gen Agent Trust Hub passed the router and notes that it reads workspace files to decide which skill to load, which is a normal surface for indirect prompt injection, so be careful in repositories you did not write.

```bash
npx skills add gamedev-skills/awesome-gamedev-agent-skills
```

Claude Code users can install one engine instead of everything, using the repository's plugin marketplace:

```bash
claude plugin marketplace add gamedev-skills/awesome-gamedev-agent-skills
claude plugin install router@awesome-gamedev-agent-skills
claude plugin install godot@awesome-gamedev-agent-skills
```

### OmniRoute ("omniroute claude")

**What it is:** [OmniRoute](https://github.com/cheaper-inference/OmniRoute) is an open-source (MIT) AI gateway, not a skill. It runs on your machine and exposes one OpenAI-compatible endpoint (`http://localhost:20128/v1` by default) in front of several hundred model providers; its repository description says 375, of which more than 150 are free. It had 86,782 GitHub stars on November 12. People searching "omniroute claude" mostly want to run Claude Code through it, so that requests fall back to another provider when a quota runs out.

**Where skills come in:** the same repository ships skills that teach an agent to operate the gateway: set it up, manage providers and API keys, build routing combos, read cost and usage logs. 46 of them are in the mcpinstall directory with 56,867 installs in total; the most installed is `cli-setup` (1,510, +25 in 24 hours).

**Good for:** developers who already use OmniRoute and want their agent to configure it instead of reading the docs.

**Not for:** anyone looking for a skill that changes how Claude Code writes code; this is infrastructure. None of its 46 skills has been through the audit scanners yet, so read the SKILL.md before installing. Keep in mind what a gateway does: your prompts and code go to whichever provider it routes to, each with its own terms and data policy, and a non-Anthropic model behind Claude Code will not behave like Claude. We have not tested output quality through it.

```bash
npm install -g omniroute
omniroute run claude --model openai/gpt-5.4
```

To add one of its skills:

```bash
npx skills add https://github.com/cheaper-inference/omniroute --skill cli-setup
```

**Two other rising names we left out.** "agent skills of oxylabs products" (up 1,500%) is a vendor pack for Oxylabs' scraping products; its most installed skill has 248 installs and none in the last 24 hours. "capafy ai agent skills" (up 1,050%) is not in the directory and we could not verify what it is.

## How to install any of these

Each command above uses the open `skills` CLI. Run it in your project root; it detects which agent you use (Claude Code, Codex, Cursor and others) and places the skill in the right folder. Some authors also publish their skill as a Claude Code plugin; check the repository README if you prefer `/plugin install`.

To try a skill without committing to it, install it in one project, use it for a day, then delete its folder. Skills are plain files, so removing them is a clean uninstall.

## Before you install: read the audit

A skill is instructions plus, sometimes, scripts that your agent will follow with your permissions. Four of the seven skills here show Warn, in each case because one or two of the five scanners raised an alert while the others passed. Some also do things you should know about: impeccable downloads a binary, taste-skill can fetch images, claude-mem sends queries to its own service. That is fine for many projects and a problem for some. Each mcpinstall skill page shows the scanner summaries, so spend thirty seconds there before installing anything into a client codebase. Of the two November 12 additions, the gamedev pack is mostly audited (71 of 80 Pass) and none of the OmniRoute skills is.

## FAQ

### What is the ponytail skill for Claude Code?

ponytail is an open-source skill by dietrichgebert that makes the agent write the smallest change that fully solves a task and explain what it skipped. It is meant to stop over-engineering. It has 122,321 installs on skills.sh as of November 8, 2026.

### Which Claude Code skill is best for frontend design?

It depends on the job. taste-skill and Uizze UI Taste set an art direction for new pages, impeccable is strongest at critique and polish of an existing UI, and Emil Kowalski's skills focus on animation and micro-interactions. Many developers combine one direction skill with one polish skill.

### Are Claude Code skills safe to install?

Most are plain instructions, but some include scripts or download tools, and they run with your agent's permissions. Check the audit result and read the SKILL.md first. On mcpinstall, each skill page lists every scanner's result and summary.

### Is OmniRoute a Claude Code skill?

No. OmniRoute is an open-source AI gateway that puts one local endpoint in front of many model providers, and you can launch Claude Code through it with `omniroute run claude`. Its repository also publishes 46 skills for operating the gateway, with 56,867 installs in the mcpinstall directory as of November 12, 2026.

### What is awesome-gamedev-agent-skills?

It is an Apache-2.0 pack of 79 game-development skills plus a router for 12 engines, including Godot, Unity, Unreal and Roblox. You install it with `npx skills add gamedev-skills/awesome-gamedev-agent-skills`; its 80 skills had 777,218 installs in the mcpinstall directory on November 12, 2026.

### Where can I find more trending skills?

The [mcpinstall skills directory](https://www.mcpinstall.com/skills) tracks installs and 24-hour trends for skills from the skills.sh registry and GitHub, with audit summaries on every page.

---

*Data: install counts and audit results from the mcpinstall skills directory (skills.sh registry data), synced November 8, 2026. Search trends: Google Trends, worldwide, past 3 months, rising related queries for "claude skills" and "agent skills". The November 12 section uses directory data and GitHub star counts from November 12, 2026; the earlier sections keep their November 8 figures.*
