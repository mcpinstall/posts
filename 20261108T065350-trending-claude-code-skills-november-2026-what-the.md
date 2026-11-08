---
title: Trending Claude Code Skills (November 2026): What They Do
date: 2026-11-08T06:53:50.241Z
---

**The Claude Code skills people are searching for in November 2026 are ponytail (write less code), taste-skill, Uizze's UI Taste, impeccable and Emil Kowalski's skills (better-looking frontends), graphify (a knowledge graph of your codebase) and claude-mem (memory across sessions). Below is what each one does, how many installs it has, what its security audit says, and the one command to install it.**

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

## How to install any of these

Each command above uses the open `skills` CLI. Run it in your project root; it detects which agent you use (Claude Code, Codex, Cursor and others) and places the skill in the right folder. Some authors also publish their skill as a Claude Code plugin; check the repository README if you prefer `/plugin install`.

To try a skill without committing to it, install it in one project, use it for a day, then delete its folder. Skills are plain files, so removing them is a clean uninstall.

## Before you install: read the audit

A skill is instructions plus, sometimes, scripts that your agent will follow with your permissions. Four of the seven skills here show Warn, in each case because one or two of the five scanners raised an alert while the others passed. Some also do things you should know about: impeccable downloads a binary, taste-skill can fetch images, claude-mem sends queries to its own service. That is fine for many projects and a problem for some. Each mcpinstall skill page shows the scanner summaries, so spend thirty seconds there before installing anything into a client codebase.

## FAQ

### What is the ponytail skill for Claude Code?

ponytail is an open-source skill by dietrichgebert that makes the agent write the smallest change that fully solves a task and explain what it skipped. It is meant to stop over-engineering. It has 122,321 installs on skills.sh as of November 8, 2026.

### Which Claude Code skill is best for frontend design?

It depends on the job. taste-skill and Uizze UI Taste set an art direction for new pages, impeccable is strongest at critique and polish of an existing UI, and Emil Kowalski's skills focus on animation and micro-interactions. Many developers combine one direction skill with one polish skill.

### Are Claude Code skills safe to install?

Most are plain instructions, but some include scripts or download tools, and they run with your agent's permissions. Check the audit result and read the SKILL.md first. On mcpinstall, each skill page lists every scanner's result and summary.

### Where can I find more trending skills?

The [mcpinstall skills directory](https://www.mcpinstall.com/skills) tracks installs and 24-hour trends for skills from the skills.sh registry and GitHub, with audit summaries on every page.

---

*Data: install counts and audit results from the mcpinstall skills directory (skills.sh registry data), synced November 8, 2026. Search trends: Google Trends, worldwide, past 3 months, rising related queries for "claude skills" and "agent skills".*
