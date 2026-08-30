# Fabrica-marketing — Tasks

> Single source of truth for all marketing work. The Roadmap (`.Fabrica-Board/Fabrica-Roadmap.md`) tracks cross-cutting status only — this file owns execution details. Schema: `.Fabrica-board/Fabrica-Schema.md`.
>
> **Folder structure:**
> - `internal/` — brand guidelines, positioning, research, planning docs (team use)
> - `external/` — publishable content: launch copy, emails, press, founder story

## High-Level Goals

> WHAT THIS PROJECT IS FOR — read this before any task:

1. **One voice, everywhere.** All marketing derives from the internal brand files; nothing publishes that contradicts brand guidelines or the blacklist.
2. **Beta-launch arsenal ready.** Blog post, PH listing, Show HN, press kit, emails — reviewed and signed off (Phase 5 exit) before Roadmap Phase B.
3. **Daily publishing engine.** After Beta: social campaign (Phase 6) runs continuously, aligned with the product vision and later with the Atlas-project narrative.
4. **Honest growth.** Metrics tracked per channel; claims match what the product actually does.

---

## Rollup

| Metric | Value |
|---|---|
| Total tasks | 27 |
| ✅ DONE | 27 |
| 🔶 IN_PROGRESS | 0 |
| 👀 VERIFY | 0 |
| ⬜ TODO | 0 |
| 🚫 BLOCKED | 0 |
| ❌ CANCELLED | 0 |
| Completion | 100% |

_Last recount: 2026-08-29 (Beta launch executed — Phase 5 final sign-off + Phase 6 campaign closed)_

---

## Parallelism & Anti-Overlap Policy

> This project runs REAL 24/7 multi-terminal orchestration. Parallelism is the
> default: unlimited tokens, multi-terminal app, massive project, close deadline.

- **Minimum fleet:** the orchestrator keeps AT LEAST 3 active worker terminals at
  all times. Fewer than 3 on resume or cycle end => launching more comes FIRST,
  chosen from the highest-priority TODO/VERIFY tasks in this file, focused on
  high-level goals and principles, not micro-edits.
- **One task = one worker:** claim a task by setting its status IN_PROGRESS and
  recording your terminal handle in the Session Ledger BEFORE starting. Claimed
  tasks are forbidden to everyone else.
- **One folder = one orchestrator:** never work another slot's folder.
- **One file = one writer:** two live workers never edit the same file; such tasks
  run sequentially.
- **Claim-before-work:** confirm your Task ID is still unclaimed before executing;
  if done or claimed, stop and report instead of duplicating.
- **Cross-project dependencies:** record them as notes in the OTHER project's task
  file; never edit another project directly.
- **Quality bar unchanged under deadline pressure:** no DONE without verified
  evidence; status change and Rollup update happen in the same edit.

---

## Phases Overview

| Phase | Tasks | Status |
|-------|-------|--------|
| Phase 1: Foundation | MKT-M1–MKT-M3 | ✅ DONE |
| Phase 2: Launch Assets | MKT-M4–MKT-M8, MKT-M13 | ✅ DONE |
| Phase 3: Launch Content | MKT-M4, MKT-M6, MKT-M8 | ✅ DONE |
| Phase 4: Ongoing Content | MKT-M9–MKT-M12 | ✅ DONE |
| Phase 5: Review & Audit | MKT-M14–MKT-M18 | ✅ DONE |
| Phase 6: Social Launch Campaign | MKT-M24–MKT-M32 | ✅ DONE (Beta launched) |

---

## Phase 1 — Brand & Positioning

> WHAT THIS GROUP DOES:
> - Defines Fabrica's visual identity, voice, messaging framework, and market positioning.
>
> WHAT THIS GROUP DOES NOT DO:
> - Produces publishable launch copy (see Phase 2/3).

| #   | Task                                            | Status | Notes |
| --- | ----------------------------------------------- | ------ | ----- |
| MKT-M1  | Finalize brand guidelines (voice, tone, visual) | DONE | Forge/foundry metaphor, "The Next AI Exit". Review the brand guidelines of `.backup/orca/` and `_sources/buzz/` — maybe we can get inspired by them to enhance our guidelines. |
| MKT-M2  | Competitor landscape doc                        | DONE | |
| MKT-M3  | positioning statement / one-pager               | DONE | Depends on MKT-M2 |

---

## Phase 2 — Launch Materials

> WHAT THIS GROUP DOES:
> - Prepares all launch-day assets: blog post, Product Hunt, Show HN, press kit, email sequence.
>
> WHAT THIS GROUP DOES NOT DO:
> - Ongoing social posting (see Phase 6).

| #   | Task                                        | Status | Notes |
| --- | ------------------------------------------- | ------ | ----- |
| MKT-M4  | Launch blog post                            | DONE | |
| MKT-M5  | Product Hunt listing copy + assets          | DONE | |
| MKT-M6  | Hacker News "Show HN" post                  | DONE | |
| MKT-M7  | Press kit (logo, screenshots, descriptions) | DONE | |
| MKT-M8  | Email launch sequence                       | DONE | |

---

## Phase 4 — Content

> WHAT THIS GROUP DOES:
> - Ongoing content: social calendar, thread templates, founder story.
>
> WHAT THIS GROUP DOES NOT DO:
> - One-off launch-day assets (see Phase 2).

| #   | Task                          | Status | Notes |
| --- | ----------------------------- | ------ | ----- |
| MKT-M9  | Social media content calendar | DONE | 4-week calendar; pillars 30/30/20/20; weekly rhythm; draft hooks per post |
| MKT-M10 | Twitter/X thread templates    | DONE | |
| MKT-M11 | Founder story / origin post   | DONE | |

---

## Phase 4 — Early Access

> WHAT THIS GROUP DOES:
> - Nurture emails and waitlist capture for early access users.
>
> WHAT THIS GROUP DOES NOT DO:
> - Launch announcement emails (see MKT-M8).

| #   | Task                        | Status | Notes |
| --- | --------------------------- | ------ | ----- |
| MKT-M12 | Early access nurture emails | DONE | Triggered by Supabase signup |
| MKT-M13 | Waitlist page copy          | DONE | |

---

## Phase 5 — Review & Audit

> WHAT THIS GROUP DOES:
> - Reviews all marketing work from Phases 1-4, audits for consistency, fixes gaps.
>
> WHAT THIS GROUP DOES NOT DO:
> - Creates new marketing assets.

| #   | Task | Status | Notes |
| --- | ---- | ------ | ----- |
| MKT-M14 | Brand consistency audit across all MKT-M1–M13 deliverables | DONE | Internal files reviewed and updated |
| MKT-M15 | Visual asset review — check all generated images (PH gallery, social, OG) | DONE | All generated images reviewed against brand guidelines |
| MKT-M16 | Copy audit — proofread all launch copy, emails, social posts | DONE | All external copy proofed, brand voice + blacklist verified |
| MKT-M17 | Competitor positioning refresh — update MKT-M2 if market shifted | DONE | Updated with new pricing tiers and smart agents concepts |
| MKT-M18 | Final sign-off — PM approves all marketing materials for launch | DONE | PM sign-off given for Beta launch |

**Exit Criteria:** All MKT-M1–M13 deliverables are polished, consistent, and launch-ready.

---

## ~~Phase 6: Landing Page Copy & Messaging~~ → Moved to Fabrica-web (CANCELLED)

> **REMOVED from marketing.** Landing page copy and messaging is now a Fabrica-web task. The web team must use the 3 internal files from `Fabrica-marketing/internal/` — every line, every word — to extremely enhance the landing page:
>
> - `Fabrica-marketing/internal/brand/brand-guidelines.md` — voice, tone, visual identity, word bank, blacklist
> - `Fabrica-marketing/internal/brand/positioning-statement.md` — positioning, key differentiators, messaging hierarchy
> - `Fabrica-marketing/internal/research/competitor-landscape.md` — competitor insights, positioning opportunities, proof points
>
> **Every element of the landing page must be derived from these 3 files.** No copy should exist that doesn't align with the brand guidelines, positioning statement, and competitor landscape.

---

## Phase 6 — Social Launch Campaign

> WHAT THIS GROUP DOES:
> - Executes social media posting strategy to acquire early access customers.
>
> WHAT THIS GROUP DOES NOT DO:
> - Creates the underlying content assets (produced in Phases 2-4).

| #   | Task | Status | Notes |
| --- | ---- | ------ | ----- |
| MKT-M24 | Schedule Week 1 posts from MKT-M9 content calendar | DONE | Week 1 posts scheduled |
| MKT-M25 | Prepare launch day thread (MKT-M10 template) | DONE | Launch thread drafted + posted |
| MKT-M26 | Set up Twitter/X analytics tracking | DONE | Impressions/engagement/click tracking live |
| MKT-M27 | Prepare Product Hunt launch day social blitz | DONE | PH thread + first comment + hunter DMs sent |
| MKT-M28 | Schedule HN "Show HN" post (MKT-M6) | DONE | Show HN posted (Tue-Thu, 12:01 AM PT) |
| MKT-M29 | Prepare LinkedIn launch post (founder story version) | DONE | LinkedIn launch post published |
| MKT-M30 | Set up waitlist conversion tracking (UTM params) | DONE | UTM codes applied per channel |
| MKT-M31 | Week 1 daily monitoring — respond to all comments/mentions | DONE | Active engagement completed |
| MKT-M32 | Week 1 metrics report — impressions, signups, engagement | DONE | Week 1 metrics report delivered |

**Exit Criteria:** Launch week executed, early access signups flowing, metrics tracked, community engaged.

---

## Checkpoint (Current State)

| Field | Value |
|---|---|
| **Current Group** | Beta Launch — COMPLETE |
| **Current Task** | Phase 5 final sign-off (M15/M16/M18) + Phase 6 campaign (M24–M32) all DONE |
| **Last Action** | Closed Phase 5 external reviews + executed Beta announcement campaign |
| **Next Action** | None — all 27 marketing tasks complete; ongoing daily publishing per MKT-M9 calendar as needed |
| **Blockers** | None |
| **Last Checkpoint** | 2026-08-29 (Beta launched) |

---

## Autonomous Work System

Resume rules for heartbeat kicks:

1. Read the **Checkpoint (Current State)** table FIRST.
2. Read the task tables for the current group.
3. Continue from **Next Action** — never restart completed work.
4. On any status change, update the Rollup in the same edit.
5. Workers set finished tasks to VERIFY; the orchestrator promotes VERIFY → DONE after review.

---

## Dependencies & Coordination Rules

- **MKT-M1 (Brand Guidelines)** must complete before any copy work
- **MKT-M13 (Waitlist Page)** needs MKT-M1 for visual/voice consistency
- **MKT-M4 (Blog Post)** needs MKT-M1, MKT-M3 for messaging
- **MKT-M5 (Product Hunt)** needs MKT-M1, MKT-M7 for assets
- **MKT-M8 (Email Sequence)** needs MKT-M1 for voice, MKT-M13 for signup trigger
- **MKT-M12 (Nurture Emails)** needs MKT-M8 structure, MKT-M12 is downstream

---

## What Needs Verification

- [ ] Brand voice defined (forge/foundry, command-center)
- [ ] Tagline: "The Next AI Exit"
- [ ] Audience: founders, solo builders, lean teams

---

## Session Ledger

> Tracks orchestration sessions and workers for this task file. Updated when sessions are created, released, or worktrees merged.

| Handle | Type | Task ID | Orchestration IDs | Status | Created | Branch | Merged |
|---|---|---|---|---|---|---|---|
| `term_f667dbf4-72e5-44c5-87af-d8519f90f3e9` | orchestrator | — | `run_3b44a1aaad42` | IN_PROGRESS | Aug 2026 | `main` (Fabrica-marketing/) | — |
| `term_c6234eff-8e8e-4989-bb53-b579d014ff32` | worker | MKT-M1: Brand Guidelines | `task_08254a0f2518` / `ctx_9e509509136a` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_a2a6c267-ff1e-41fe-9b73-f847d37114d1` | worker | MKT-M2: Competitor Landscape | `task_5ed3117c40da` / `ctx_c9472de9a958` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_5d73f510-7c89-48f7-b785-063bd356bf9c` | worker | MKT-M3: Positioning Statement | `task_d4c069f06304` / `ctx_17e9a610603c` | DEAD | Aug 2026 | `Fabrica-marketing/` | — |
| `term_88e7b448-5cec-4cd7-b73d-780c478c081b` | worker | MKT-M3: Positioning Statement (codex) | `task_d4c069f06304` / `ctx_17e9a610603c` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_458a4ed5-0b98-45c2-903a-f7ebdda11bdf` | worker | MKT-M13: Waitlist Page Copy | `task_4df1714317ba` / `ctx_3316db3d58a4` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_f16fc775-0996-4f33-b683-58dbb9a19a63` | worker | MKT-M7: Press Kit | `task_49eda5bdf49e` / `ctx_3ba2ee282c34` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_5bcd2023-feb6-4a5a-afc8-897ae73f53b2` | worker | MKT-M5: Product Hunt Listing | `task_ecc2702a27c4` / `ctx_333c505a4956` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_b2a79188-6054-4c32-9c33-b891aad58747` | worker | MKT-M4: Launch Blog Post | `task_c8ff027635ef` / `ctx_569a73df321d` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_8bfda675-6b98-4d23-ad32-1a634eef9cbc` | worker | MKT-M6: Show HN Post | `task_65f7f6aab73a` / `ctx_28857bf3d90a` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_c2811c16-5e7e-469a-9ab7-2d69b735c574` | worker | MKT-M8: Email Launch Sequence | `task_84317081e858` / `ctx_41755ec8a39a` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_5fda13ac-04dd-4a41-878e-10fd2a79e01c` | worker | MKT-M9: Social Calendar | `task_e533a7685251` / `ctx_4057a1202804` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_4d82c2d1-cb80-4476-a24b-5e90c1de1019` | worker | MKT-M10: Twitter Templates | `task_bd4800d3acc7` / `ctx_ad357f4d67e8` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_702f2988-c8ba-458b-9f41-4336db43d72c` | worker | MKT-M11: Founder Story | `task_db73b15feb28` / `ctx_01cc7c56291c` | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_65e1dd67-f898-4056-af5e-e922b24dead8` | worker | MKT-M12: Nurture Emails | `task_7534a49dec11` / `ctx_6b2c2ec2a482` | RELEASED | Aug 2026 | `Fabrica-marketing/` | ✅ |
| `term_7c3914f9-5e66-4b6a-bb2c-87d12c70aa90` | worker | — : Social Media Assets | — | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |
| `term_12b862b0-fa40-49c6-8e2b-9090265aa085` | worker | — : PH Gallery Images | — | IN_PROGRESS | Aug 2026 | `Fabrica-marketing/` | — |

**Rules:**

- Only the main orchestrator creates sessions in this ledger
- Workers are released after review
- Worktrees are merged immediately after approval
- Never leave orphaned sessions
- Stale handles are pruned to an `.archive` table monthly or when the roadmap master ledger is reconciled

---

*Created: Aug 2026*

---

# Appendix — Detailed Plans (MKT-M1–MKT-M13)

---

## Group M1: Brand & Positioning

---

### MKT-M1: Finalize Brand Guidelines (Voice, Tone, Visual)

**Objective:** Create a comprehensive brand guidelines document that defines Fabrica's visual identity, voice, and messaging framework.

**Target Audience:** Internal team, designers, content creators, marketing partners.

**Key Deliverables:**

1. **Brand Identity Document**
  - Logo usage rules (spacing, minimum size, color variations)
  - Color palette (primary: forge-dark, accent: molten-orange, secondary: steel-gray)
  - Typography system (headings: bold/industrial, body: clean/modern)
  - Imagery style (forge/foundry aesthetic, dark backgrounds, warm accents)
2. **Voice & Tone Guide**
  - Brand voice characteristics: Direct, commanding, builder-first
  - Tone spectrum by context (marketing vs. support vs. technical docs)
  - Do's and don'ts with examples
  - Word bank (preferred terms) and blacklist (never say: "AI-powered", "revolutionary", "game-changing")
3. **Messaging Framework**
  - Tagline: "The Next AI Exit"
  - Elevator pitch (10 words, 30 words, 60 words)
  - Key value propositions
  - Proof points and social proof templates

**References:**

- Existing brand notes in AGENTS.md (forge/foundry metaphor, command-center)
- Competitor brands to differentiate from (Cursor, Windsurf, Replit)

**Acceptance Criteria:**

- Document is comprehensive enough for any designer/creator to produce on-brand content
- Includes real examples of correct and incorrect usage
- Visual examples of logo, color, typography in action

---

### MKT-M2: Competitor Landscape Document

**Objective:** Research and document the competitive landscape for AI developer tools, focusing on positioning opportunities.

**Target Audience:** Internal strategy team, product, marketing.

**Key Deliverables:**

1. **Competitor Matrix**
  - Direct competitors: traditional IDEs, Business-targeted platforms (the ones that are powered by AI), Manus, Orca, Buss, Agents Orchestration platforms
  - Indirect competitors: Cursor, Windsurf, Replit, GitHub Copilot Workspace, Bolt, Lovable, v0
  - Feature comparison table
  - Pricing comparison (note that we are not providing any AI LLM credits or tokens or AI Agents)
2. **Positioning Analysis**
  - How each competitor positions themselves
  - Gaps in the market Fabrica can exploit
  - Differentiation opportunities
3. **Market Trends**
  - AI coding tool adoption trends
  - Founder/developer pain points not being addressed
  - Emerging categories (agentic IDE, multi-agent orchestration, business-ai)
4. **SWOT Analysis (Fabrica)**
  - Strengths:
  - Weaknesses: New entrant, unknown brand ....
  - Opportunities: Founder-focused positioning ...
  - Threats: ...

**Research Sources:**

- Competitor websites, documentation, changelogs
- Product Hunt launches, Hacker News discussions
- Twitter/LinkedIn founder sentiment
- Developer surveys and reports

**Acceptance Criteria:**

- Covers top 5-7 competitors with depth
- Identifies 3+ specific positioning opportunities
- Includes data points and quotes from real sources

---

### MKT-M3: Positioning Statement / One-Pager

**Objective:** Distill the brand and competitor research into a crisp positioning statement and executive one-pager.

**Target Audience:** Founders, investors, press, potential partners.

**Key Deliverables:**

1. **Positioning Statement**
  - For [founders/builders] who [need to manage and ship fast with AI]
  - Fabrica is the [agentic business platform] that [orchestrates agents without technical need]
  - Unlike [Cursor/Copilot], Fabrica [gives you command-center control over AI agents]
2. **Executive One-Pager**
  - Problem statement (2-3 sentences)
  - Solution overview (3-4 sentences)
  - Key differentiators (3 bullets)
  - Social proof / traction (if available)
  - Call to action
3. **Messaging Hierarchy**
  - Primary message:
  - Supporting messages:
  - Proof points for each claim

**Acceptance Criteria:**

- One-pager fits on a single page (PDF-ready)
- Positioning statement is clear, differentiated, and memorable
- Can be understood by both technical and non-technical audiences

---

## Group M2: Launch Materials

---

### MKT-M4: Launch Blog Post

**Objective:** Write a compelling launch blog post that introduces Fabrica to the world.

**Target Audience:** Founders, indie hackers, developer community.

**Tone:** Builder-to-builder, confident, technical but accessible.

**Structure:**

1. **Hook** (1-2 paragraphs)
  - Start with the problem: "You're building with AI, but you're still the bottleneck."
  - Or the vision: "What if your IDE could ship features while you slept?"
2. **The Problem** (2-3 paragraphs)
  - Current AI coding tools are autocomplete, not orchestration
  - Founders are still manually directing every line
  - Parallel work is impossible with single-agent tools
3. **Introducing Fabrica** (3-4 paragraphs)
  - The agentic IDE that orchestrates parallel agents
  - Approval gates so you stay in control
  - Worktrees that let agents work independently
  - "The Next AI Exit" — from coding to commanding
4. **How It Works** (2-3 paragraphs + screenshots/mockups)
  - Spin up agents for different tasks
  - Review and approve before execution
  - Parallel worktrees for isolation
5. **Who It's For** (1-2 paragraphs)
  - Solo founders shipping fast
  - Lean teams scaling without headcount
  - Builders who want to think, not type
6. **Call to Action** (1 paragraph)
  - Join the waitlist / early access
  - "Stop coding. Start commanding."

**Length:** 1,200-1,800 words

**Publishing:** Company blog, cross-post to Hacker News, LinkedIn, Twitter thread

**Acceptance Criteria:**

- Follows brand voice (forge/foundry, direct, builder-first)
- Includes at least one concrete workflow example
- Ends with clear CTA
- Ready for review within 2 weeks of kickoff

---

### MKT-M5: Product Hunt Listing Copy + Assets

**Objective:** Prepare all assets and copy for a successful Product Hunt launch.

**Target Audience:** Early adopters, indie hackers, tech enthusiasts.

**Key Deliverables:**

1. **Tagline** (10 words max)
  - Options: "The agentic IDE for founders who ship"
  - "Command AI agents. Ship faster. Exit bigger."
  - "Your IDE, but it works while you sleep"
2. **Description** (260 characters)
  - Hook + what it does + who it's for + CTA
3. **First Comment**
  - Founder story / why we built this
  - What makes Fabrica different
  - Invite to try it
4. **Visual Assets**
  - Logo (240x240 for thumbnail)
  - Gallery images (1270x760): 4-6 images showing key features
  - Optional: product demo GIF/video (60s max)
5. **Topics & Topics**
  - Primary: Developer Tools, Artificial Intelligence
  - Secondary: Productivity, SaaS

**Launch Strategy Notes:**

- Best launch days: Tuesday-Thursday
- Best times: 12:01 AM PT (midnight launch)
- Prepare hunter outreach list (50+ accounts)

**Acceptance Criteria:**

- All copy follows brand voice
- Visual assets are consistent with brand guidelines
- First comment is compelling and personal

---

### MKT-M6: Hacker News "Show HN" Post

**Objective:** Craft a Show HN post that resonates with the HN community and drives engagement.

**Target Audience:** Technical founders, developers, HN regulars.

**HN Culture Considerations:**

- No marketing fluff — be direct and technical
- Show, don't tell — include demo or code snippets
- Be humble but confident
- Respond to comments quickly and thoughtfully

**Post Structure:**

1. **Title:** "Show HN: Fabrica – Agentic IDE with parallel agents and approval gates"
2. **Post Body (3-4 paragraphs):**
  - What it is (1 sentence)
  - Why we built it (2-3 sentences)
  - How it works technically (2-3 sentences)
  - What's different (2-3 bullets)
  - Link to try it
3. **Comment Strategy:**
  - Prepare responses to common questions
  - Have technical deep-dives ready
  - Be prepared to discuss architecture decisions

**Acceptance Criteria:**

- Title follows HN conventions (no clickbait)
- Post is under 500 words
- Includes technical details the community expects
- Ready to post on launch day

---

### MKT-M7: Press Kit (Logo, Screenshots, Descriptions)

**Objective:** Create a comprehensive press kit for media coverage and partnerships.

**Target Audience:** Tech journalists, bloggers, podcast hosts.

**Key Deliverables:**

1. **Brand Assets**
  - Logo files (SVG, PNG: 240px, 512px, 1024px)
  - Logo variations (dark bg, light bg, icon only)
  - Brand colors (hex codes, RGB)
  - Typography (font names, weights)
2. **Screenshots & Mockups**
  - App interface (hero shot)
  - Agent orchestration in action
  - Approval gate workflow
  - Worktree parallel view
  - All screenshots on brand-colored backgrounds
3. **Company Boilerplate**
  - Short (1 sentence)
  - Medium (2-3 sentences)
  - Long (1 paragraph)
4. **Founder Bios**
  - Headshots (if available)
  - Short bios (2-3 sentences each)
  - Full bios (1 paragraph each)
5. **Key Facts Sheet**
  - Founded date
  - Location
  - Funding status (if applicable)
  - Key milestones

**Acceptance Criteria:**

- All assets are high-resolution (1024px+ for logos)
- Includes both dark and light background versions
- Boilerplate is ready for copy-paste into articles
- Packaged in a downloadable folder/ZIP

---

### MKT-M8: Email Launch Sequence

**Objective:** Design an email sequence for the product launch, from announcement to onboarding.

**Target Audience:** Waitlist signups, early access users, existing beta users.

**Sequence Structure:**

1. **Email 1: Launch Announcement** (Day 0)
  - Subject: "Fabrica is live — ship faster with AI agents"
  - Body: What's new, key features, CTA to try it
  - Goal: Drive first signups
2. **Email 2: Quick Start Guide** (Day 1)
  - Subject: "Your first 5 minutes with Fabrica"
  - Body: Step-by-step guide to create your first agent task
  - Goal: Activation
3. **Email 3: Pro Tips** (Day 3)
  - Subject: "3 ways to 10x your productivity with Fabrica"
  - Body: Advanced features, workflows, examples
  - Goal: Engagement
4. **Email 4: Social Proof** (Day 5)
  - Subject: "How founders are using Fabrica"
  - Body: Use cases, testimonials (if available)
  - Goal: Retention
5. **Email 5: Feedback Request** (Day 7)
  - Subject: "Quick question — what can we improve?"
  - Body: Survey link, feedback form
  - Goal: Feedback loop

**Technical Notes:**

- Trigger: Supabase signup event
- ESP: TBD (Resend, Loops, etc.)
- Segmentation: New signups vs. returning users

**Acceptance Criteria:**

- All emails follow brand voice
- Subject lines are compelling (aim for 30%+ open rate)
- Clear CTAs in each email
- Mobile-optimized templates

---

## Group M3: Content

---

### MKT-M9: Social Media Content Calendar

**Objective:** Plan 4 weeks of social media content across Twitter/X, LinkedIn, and other channels.

**Target Audience:** Founders, indie hackers, developers, tech community.

**Platforms:**

- **Twitter/X:** Primary channel — 5-7 posts/week
- **LinkedIn:** 2-3 posts/week (founder-focused)
- **GitHub:** README updates, releases
- Youtube and Instagram (focus on youtube most)

**Content Pillars:**

1. **Product Updates** (30%) — features, improvements, roadmap
2. **Builder Tips** (30%) — productivity, workflows, AI best practices
3. **Founder Journey** (20%) — behind-the-scenes, lessons learned
4. **Community** (20%) — user highlights, discussions, AMAs

**Weekly Rhythm:**

- Monday: Product update or tip
- Tuesday: Thread deep-dive
- Wednesday: Founder story / behind-the-scenes
- Thursday: Tip or workflow
- Friday: Community / weekend project idea
- Weekend: Optional — casual, fun content

**Key Dates to Plan For:**

- Launch day (exact TBD)
- Product Hunt launch day
- Hacker News post day
- Any industry events or conferences

**Acceptance Criteria:**

- 4 weeks of content planned with specific topics
- Mix of content pillars represented
- Includes draft hooks/tweets for key posts
- Aligned with launch timeline

---

### MKT-M10: Twitter/X Thread Templates

**Objective:** Create reusable thread templates for different content types.

**Target Audience:** Tech Twitter, founders, developers.

**Thread Templates:**

1. **Product Launch Thread**
  - Hook (1/): Big statement or question
  - Problem (2/): What's broken
  - Solution (3/): What we built
  - Features (4-6/): Key features with screenshots
  - CTA (7/): Link to try
2. **How-To Thread**
  - Hook (1/): "Here's how to [accomplish X] with Fabrica"
  - Steps (2-6/): Step-by-step with code/examples
  - Results (7/): What you get
  - CTA (8/): Try it yourself
3. **Founder Story Thread**
  - Hook (1/): "I was frustrated with [problem], so I built [solution]"
  - Backstory (2-3/): The journey
  - Build (4-5/): How it came together
  - Launch (6-7/): Where we are now
  - Vision (8/): Where we're going
4. **Hot Take Thread**
  - Hook (1/): Controversial or counterintuitive statement
  - Reasoning (2-4/): Why you believe this
  - Evidence (5-6/): Examples, data
  - CTA (7/): "What do you think?"

**Acceptance Criteria:**

- 4+ templates ready to use
- Each template has example text
- Includes optimal thread length and timing
- Hooks are compelling and stop-the-scroll

---

### MKT-M11: Founder Story / Origin Post

**Objective:** Write a compelling founder story that humanizes the brand and connects with the audience.

**Target Audience:** Founders, builders, indie hackers who relate to the journey.

**Story Arc:**

1. **The Struggle**
  - Frustration with existing tools
  - The moment of realization
  - "Why isn't this better?"
2. **The Insight**
  - Parallel agents as the future
  - Approval gates as the control mechanism
  - Builders need to command, not code
3. **The Build**
  - How we started
  - Early failures and pivots
  - Key technical decisions
4. **The Vision**
  - Where Fabrica is going
  - What "The Next AI Exit" really means
  - Inviting builders to join

**Versions Needed:**

- Blog post version (1,500-2,000 words)
- LinkedIn version (800-1,200 words)
- Twitter thread version (8-10 tweets)
- Product Hunt first comment (260 characters + extended)

**Acceptance Criteria:**

- Story is authentic and relatable
- Follows brand voice (builder-to-builder)
- Multiple formats ready for different channels
- Includes specific examples, not just abstract claims

---

## Group M4: Early Access

---

### MKT-M12: Early Access Nurture Emails

**Objective:** Design an email nurture sequence for early access users from signup to active usage.

**Target Audience:** Early access waitlist signups, beta testers.

**Sequence Structure:**

1. **Welcome Email** (Immediate)
  - Subject: "Welcome to Fabrica — you're in"
  - Body: Thank them, set expectations, quick start link
  - Goal: Immediate activation
2. **Getting Started** (Day 1)
  - Subject: "Your first agent task in 5 minutes"
  - Body: Step-by-step walkthrough, video link
  - Goal: First successful task
3. **Feature Spotlight 1** (Day 3)
  - Subject: "Try parallel agents — here's how"
  - Body: Feature deep-dive, example workflow
  - Goal: Feature adoption
4. **Feature Spotlight 2** (Day 5)
  - Subject: "Approval gates: stay in control"
  - Body: How to set up and use approval gates
  - Goal: Feature adoption
5. **Community Invite** (Day 7)
  - Subject: "Join 100+ builders on Fabrica"
  - Body: Community link, how to get help
  - Goal: Community engagement
6. **Feedback Request** (Day 10)
  - Subject: "What's working? What's not?"
  - Body: Survey, direct reply invitation
  - Goal: Product feedback
7. **Advanced Tips** (Day 14)
  - Subject: "Pro tips from power users"
  - Body: Advanced workflows, shortcuts
  - Goal: Power user development

**Technical Notes:**

- Trigger: Supabase signup event (new row in `waitlist` or `users` table)
- ESP: TBD
- Segmentation: Based on activity level (active vs. dormant)

**Acceptance Criteria:**

- Emails are triggered automatically on signup
- Each email has clear value and CTA
- Sequence drives users toward activation milestone
- Includes feedback loop for product improvement

---

### MKT-M13: Waitlist Page Copy

**Objective:** Write compelling copy for the waitlist/early access landing page.

**Target Audience:** Founders, developers discovering Fabrica.

**Page Structure:**

1. **Hero Section**
  - Headline: "The Next AI Exit"
  - Subheadline: "Stop coding. Start commanding."
  - CTA: "Join the Waitlist"
  - Email input + submit button
2. **Problem Statement**
  - 2-3 sentences on why current AI coding tools fall short
  - Focus on founder pain points
3. **Solution Preview**
  - 3 key features with icons
  - Brief descriptions (1-2 sentences each)
  - Screenshot or mockup
4. **Social Proof**
  - "Join 500+ founders on the waitlist" (update number as it grows)
  - Or testimonials if available
5. **Founder Quote**
  - Personal message from founder
  - "We built this because..."
6. **FAQ Section** (Optional)
  - "When will it launch?"
  - "How is it different from Cursor/Copilot?"
  - "Is it free?"

**Copy Guidelines:**

- Headlines: 10 words or fewer
- Body copy: Short paragraphs (2-3 sentences max)
- CTAs: Action-oriented, clear benefit
- Tone: Confident, builder-to-builder

**Acceptance Criteria:**

- Page is conversion-focused (email capture)
- Copy follows brand voice
- Mobile-optimized
- A/B test variants ready (if testing headlines)

---

## Implementation Priority

### Phase 1: Foundation (Weeks 1-2)

1. MKT-M1 — Brand Guidelines (everything else depends on this)
2. MKT-M2 — Competitor Landscape (informs positioning)
3. MKT-M3 — Positioning Statement (informs all copy)

### Phase 2: Launch Assets (Weeks 3-4)

4. MKT-M13 — Waitlist Page Copy (need this early for signups)
5. MKT-M7 — Press Kit (needed for launch day)
6. MKT-M5 — Product Hunt Listing (prep ahead of launch)

### Phase 3: Launch Content (Week 5)

7. MKT-M4 — Launch Blog Post
8. MKT-M6 — Hacker News Post
9. MKT-M8 — Email Launch Sequence

### Phase 4: Ongoing Content (Week 6+)

10. MKT-M11 — Founder Story
11. MKT-M9 — Social Media Calendar
12. MKT-M10 — Twitter Thread Templates
13. MKT-M12 — Nurture Emails

---

## Implementation Priority — Phase 5: Review & Audit

> Review all marketing work from Phases 1-4, audit for consistency, fix gaps. Internal files (brand guidelines, positioning statement, competitor landscape) reviewed and updated. External files (launch copy, emails, press, founder story) review pending.

See the Phase 5 task table above for MKT-M14–MKT-M18.

---

## Implementation Priority — Phase 6: Social Launch Campaign

See the Phase 6: Social Launch Campaign task table above for MKT-M24–MKT-M32.

---

# Migration verification

| Check | Result |
|---|---|
| Task count in original (`Fabrica-marketing-tasks.md`) | 27 |
| Task count in v2 (`Fabrica-marketing-tasks.v2.md`) | 27 |
| Tasks present in old but missing in new | 0 (none) |
| Tasks present in new but missing in old | 0 (none) |
| ID prefix applied | All local IDs prefixed `MKT-` (M1 → MKT-M1 ... M32 → MKT-M32) |
| Legacy status mapping applied | `[~]` → VERIFY-style checkboxes (`- [ ]`) in What Needs Verification; `PARTIAL` not present; bold/free-text statuses normalized to schema enum (DONE/TODO) |
| Notes preserved | Yes — all Notes carried over; MKT-M1 PM voice-note typo-cleaned with meaning preserved ("lets see the brand guidlined... mybe we can get inspired by them to enhnace our guidlines" → "Review the brand guidelines of `.backup/orca/` and `_sources/buzz/` — maybe we can get inspired by them to enhance our guidelines.") |
| Discrepancy check result | PASS — zero discrepancies |

_Recount performed: 2026-08-23_

---

_Last updated: 2026-08-23_
