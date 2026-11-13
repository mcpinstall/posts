---
title: Best Claude Skills for Web Design: 11 Compared (Nov 2026)
date: 2026-11-13T16:29:12.963Z
---

The best Claude skills for website design, as of November 13, 2026: Anthropic's **frontend-design** for a new site that should not look templated (1.1 million installs, plain instructions, nothing to run); **impeccable** or Uizze's **UI Taste** to fix an existing site that looks AI-made; Emil Kowalski's **emil-design-eng** for animation and polish; and Vercel's **web-design-guidelines** to review what you built. If you want the most opinionated option and can run Node and Chrome locally, **taste-skill** (design-taste-frontend) art-directs the page and screenshots its own work.

We compared 11 design skills from the MCPInstall directory's [frontend and design category](https://www.mcpinstall.com/skills/category/frontend-design) (7,803 skills, 30.3 million installs). For each one we took install counts from skills.sh, the five security audits our directory tracks, and we read every SKILL.md to see what it tells your agent to do and what it runs on your machine. We did not score the visual output; taste is yours to judge.

## Which Claude design skill should you install?

| Skill | Publisher | Installs (total) | Installs (last week) | Best for | What it runs on your machine | Audits |
|---|---|---|---|---|---|---|
| [frontend-design](https://www.mcpinstall.com/skills/anthropics/skills/frontend-design) | Anthropic | 1,113,813 | 30,893 | A distinctive new page or site | Nothing: 71 lines of instructions | 5 of 5 clean |
| [UI Taste](https://www.mcpinstall.com/skills/site/uizze.sh/ui-taste) | Uizze | 1.1M | 161,684 | Removing "AI slop" from web and iOS UI | Nothing; optional paid Uizze MCP | Not audited yet |
| [web-design-guidelines](https://www.mcpinstall.com/skills/vercel-labs/agent-skills/web-design-guidelines) | Vercel | 858,213 | 26,986 | Reviewing UI and accessibility | Fetches its rules from GitHub on every run | 4 clean, Socket warning |
| [design-taste-frontend](https://www.mcpinstall.com/skills/leonxlnx/taste-skill/design-taste-frontend) | taste-skill | 772,371 | 34,196 | Art-directed landing and marketing pages | Node 22 scripts plus a local Chrome inspector | Snyk medium (1 issue), Socket warning, Runlayer low |
| [ui-ux-pro-max](https://www.mcpinstall.com/skills/nextlevelbuilder/ui-ux-pro-max-skill/ui-ux-pro-max) | nextlevelbuilder | 449,750 | 12,691 | Picking a style, palette and font pairing | A Python search script over bundled CSV data | ZeroLeaks low (1 finding) |
| [emil-design-eng](https://www.mcpinstall.com/skills/emilkowalski/skills/emil-design-eng) | Emil Kowalski | 444,763 | 20,798 | Animation decisions and UI polish | Nothing: 674 lines of instructions | 3 of 3 clean |
| [impeccable](https://www.mcpinstall.com/skills/pbakaus/impeccable/impeccable) | Paul Bakaus | 405,604 | 15,779 | Redesigning, critiquing and polishing existing UI | A bundled launcher script; 60 files | Socket warning |
| [uizze-ui](https://www.mcpinstall.com/skills/uizze/uizze/uizze-ui) | Uizze | 367,467 | 96,498 | Building UI with real-product references | Nothing; uses the Uizze MCP if connected (paid) | 3 of 3 clean |
| [apple-design](https://www.mcpinstall.com/skills/emilkowalski/skills/apple-design) | Emil Kowalski | 296,841 | 18,972 | Gestures, springs and fluid motion | Nothing: instructions | 3 of 3 clean |
| [shadcn](https://www.mcpinstall.com/skills/shadcn-ui/ui/shadcn) | shadcn/ui | 146,947 | 8,436 | Building with shadcn/ui components | `npx shadcn@latest` | Socket warning |
| [image-to-ui](https://www.mcpinstall.com/skills/uizze/uizze/image-to-ui) | Uizze | 138,679 | 32,363 | Turning a screenshot or mockup into code | Nothing; asks the agent to render and compare | 3 of 3 clean |

Installs are skills.sh counts read on November 13, 2026; "last week" is the most recent week in skills.sh's weekly chart. Audit verdicts come from Gen Agent Trust Hub, Socket, Snyk, Runlayer and ZeroLeaks as stored in our directory; not every provider has scanned every skill.

## Which skill makes a new site look designed, not templated?

**frontend-design (Anthropic)** is the default for a reason: it is the most installed design skill and the lightest. Its 71 lines cast the agent as the design lead of a studio whose client "has already rejected proposals that felt cliché or templated", then make it ground choices in the subject, plan, critique against the brief and show restraint. It runs nothing and passes all five audits. Choose it when you want better defaults without adopting anyone's house style.

**design-taste-frontend (taste-skill v2)** goes much further. Its description says it was distilled from a study of 247 curated gallery sites, and it deals the agent a researched direction (idea, composition, type, palette, photographs, section blueprints) from a bundled library. Then it builds the page, runs a real-browser inspector that fails pages breaking its rules (for example fewer than four real pictures, or two near-empty screens), screenshots at three widths and fixes the top problems for up to three rounds. The cost: it needs Node 22 and an installed Chrome, Edge or Chromium, one script needs network access, and it carries 37 files. Snyk reports one medium-severity issue and Socket an AI-flagged anomaly, as of October 20, 2026; read both on the skill page before installing.

**ui-ux-pro-max** is a reference library rather than an art director: a Python search script (no external dependencies) over bundled data with 79 styles, 192 product palettes, 74 font pairings and 119 UX guidelines. It is useful when you know the product type and want a starting design system quickly.

## Which skill fixes a site that already looks AI-made?

**impeccable** (version 4.7.0) is built for this: design, redesign, critique, audit, polish, simplify, harden or animate an existing interface, including native iOS and Android screens. It scales ambition to the page's mode, bold on marketing and experience pages, restrained on screens people operate or read. It runs a bundled launcher script once per session to load project context, and ships 60 files of playbooks and sub-agents.

**UI Taste by Uizze** targets the same "AI slop" problem with less machinery: the free skill works "without an account, token, MCP connection, dependency, script, or executable", per its SKILL.md. It had the most installs of any skill in this list last week (161,684). It is new enough that none of the five scanners in our directory has audited it yet. Uizze's paid MCP adds real-product references, and its sibling **uizze-ui** leans on that MCP more directly.

## Which skill is best for animation and polish?

Emil Kowalski's skills. **emil-design-eng** encodes his approach to UI polish: an animation decision framework (should this animate, what is the purpose, which easing, how fast), spring animations, perceived performance and a required review format. **apple-design** translates Apple's WWDC design talks for the web: response, direct manipulation, interruptibility, springs, momentum and rubber-banding. Both are pure instructions with clean audits. The same author publishes smaller companions in the directory, such as improve-animations and find-animation-opportunities.

## Which skill reviews a design you already built?

**web-design-guidelines (Vercel)** is a 39-line skill that tells the agent to fetch Vercel's Web Interface Guidelines from GitHub before each review and report findings as `file:line`. The upside is that the rules stay current. The trade-off is that what your agent follows can change between runs without the skill's version changing, the same trust question we covered for [MCP servers whose dependencies drift](https://www.mcpinstall.com/blog/mcp-server-supply-chain-security).

**image-to-ui** covers the other review case: matching a reference. It asks the agent to render the result at the reference's viewport, compare side by side and correct layout, then type, spacing and colour, and to say the match is unverified if it cannot render.

## Why don't total installs tell you what people use now?

Because skills get retired and renamed while their counters keep growing. taste-skill's **high-end-visual-design** shows 458,894 installs and **minimalist-ui** 425,166, but last week they had 26 and 23. Both now describe themselves as "classic" style skills that run only when called by name; their work moved into design-taste-frontend. Meanwhile uizze-ui, at 367,467 total, added 96,498 installs in a single week. Look at the weekly line on skills.sh, or the trending rank on our skill pages, before trusting a big total.

## What should you check before installing a design skill?

- **What it runs.** Six of the eleven are instructions only. Three run bundled scripts (Node, Python or a launcher), one fetches its rules at run time, and the shadcn skill is allowed to run `npx shadcn@latest`, which pulls the newest CLI release each time.
- **Its audits, and their dates.** Our skill pages show each scanner's verdict with the date it ran. A warning is a reason to read the flagged file, not an automatic no. Our analysis of [4,936 agent skill audits](https://www.mcpinstall.com/blog/agent-skills-security) explains what each scanner looks for.
- **Whether it fights your design system.** Opinionated skills such as design-taste-frontend impose a house style. If you already have tokens and components, a review skill or impeccable's polish mode will respect them more.

## When don't you need a design skill?

- **Internal tools and admin screens** where consistency matters more than character: a component library (shadcn/ui, your own) gets you further than art direction.
- **When a designer hands you a finished file.** Use image-to-ui or the Figma integration you already have; an art-direction skill will argue with the design.
- **One-off prototypes.** The default model behaviour is fine for throwaway work; skills pay off on pages people will judge.

## How to install them

Every skill above installs with the skills CLI, for example `npx skills add https://github.com/anthropics/skills --skill frontend-design`; the skill pages linked in the table show the command for each. For shadcn, use `npx skills add https://github.com/shadcn-ui/ui --skill shadcn`. For what is gaining installs this month, see [trending Claude Code skills for November 2026](https://www.mcpinstall.com/blog/trending-claude-code-skills-november-2026), and browse the full [frontend and design skills directory](https://www.mcpinstall.com/skills/category/frontend-design) for the rest of the category.

## How we compared

- **Candidates:** the most installed skills in MCPInstall's frontend-design category, keeping those about visual web design and dropping unrelated entries (project setup, video tooling, React performance). Retired skills are discussed but not listed.
- **Installs:** skills.sh skill pages, read November 13, 2026 (UI Taste's total is the rounded figure skills.sh shows).
- **Audits:** the five providers our directory syncs; dates are on each skill page. UI Taste has none yet.
- **Behaviour:** read from each SKILL.md and its file list. We did not run the skills or rate their output.

## FAQ

### What is the best Claude skill for website design?

For most people, Anthropic's frontend-design: it is the most installed design skill, has no scripts and passes all five audits. Add impeccable or UI Taste when you are fixing an existing site, and emil-design-eng when motion matters.

### Is taste-skill better than impeccable?

They aim at different jobs. taste-skill (design-taste-frontend) art-directs new public-facing pages with its own house style and checks them in a browser. impeccable is broader and better suited to redesigning, critiquing and polishing interfaces that already exist, including app screens.

### Do Claude design skills work in Cursor or Codex?

Most do. They are SKILL.md files, and the skills CLI installs them for several agents. UI Taste says it is designed for Claude Code, Codex, Cursor and GitHub Copilot, and several skills here ship an OpenAI agent config alongside the SKILL.md.

### Are design skills safe to install?

Six of the eleven contain only instructions. The others run scripts or fetch content, which is normal for what they do but worth reading first. Check the audit verdicts on the skill page and what the skill is allowed to run.

### Which design skill is growing fastest?

By last week's installs on skills.sh: UI Taste (161,684), then uizze-ui (96,498), taste-skill's design-taste-frontend (34,196) and image-to-ui (32,363).

## Sources

Checked November 13, 2026.

- skills.sh skill pages for each skill (install totals and weekly installs)
- Each skill's SKILL.md and file list, as synced to the [MCPInstall skills directory](https://www.mcpinstall.com/skills)
- Audit verdicts from Gen Agent Trust Hub, Socket, Snyk, Runlayer and ZeroLeaks, as shown on each MCPInstall skill page
